---
name: "ms365-tenant-manager"
description: "Microsoft 365 tenant administration for Global Administrators. Triggers: 'use ms365-tenant-manager', 'ms365 tenant manager', 'ms365-tenant-manager task'."
allowed-tools: Bash, Glob, Grep, Read
---

# Microsoft 365 Tenant Manager

Expert guidance and automation for Microsoft 365 Global Administrators managing tenant setup, user lifecycle, security policies, and organizational optimization.

---

## Quick Start

### Run a Security Audit

```powershell
Connect-MgGraph -Scopes "Directory.Read.All","Policy.Read.All","AuditLog.Read.All"
Get-MgSubscribedSku | Select-Object SkuPartNumber, ConsumedUnits, @{N="Total";E={$_.PrepaidUnits.Enabled}}
Get-MgPolicyAuthorizationPolicy | Select-Object AllowInvitesFrom, DefaultUserRolePermissions
```

### Bulk Provision Users from CSV

```powershell
# CSV columns: DisplayName, UserPrincipalName, Department, LicenseSku
Import-Csv .\new_users.csv | ForEach-Object {
    $passwordProfile = @{ Password = (New-Guid).ToString().Substring(0,16) + "!"; ForceChangePasswordNextSignIn = $true }
    New-MgUser -DisplayName $_.DisplayName -UserPrincipalName $_.UserPrincipalName `
               -Department $_.Department -AccountEnabled -PasswordProfile $passwordProfile
}
```

### Create a Conditional Access Policy (MFA for Admins)

```powershell
$adminRoles = (Get-MgDirectoryRole | Where-Object { $_.DisplayName -match "Admin" }).Id
$policy = @{
    DisplayName = "Require MFA for Admins"
    State = "enabledForReportingButNotEnforced"   # Start in report-only mode
    Conditions = @{ Users = @{ IncludeRoles = $adminRoles } }
    GrantControls = @{ Operator = "OR"; BuiltInControls = @("mfa") }
}
New-MgIdentityConditionalAccessPolicy -BodyParameter $policy
```

---

## Workflows

### Workflow 1: New Tenant Setup

**Step 1: Generate Setup Checklist**

Confirm prerequisites before provisioning:
- Global Admin account created and secured with MFA
- Custom domain purchased and accessible for DNS edits
- License SKUs confirmed (E3 vs E5 feature requirements noted)

**Step 2: Configure and Verify DNS Records**

```powershell
# After adding the domain in the M365 admin center, verify propagation before proceeding
$domain = "company.com"
Resolve-DnsName -Name "_msdcs.$domain" -Type NS -ErrorAction SilentlyContinue
# Also run from a shell prompt:
# nslookup -type=MX company.com
# nslookup -type=TXT company.com   # confirm SPF record
```

Wait for DNS propagation (up to 48 h) before bulk user creation.

**Step 3: Apply Security Baseline**

```powershell
# Disable legacy authentication (blocks Basic Auth protocols)
$policy = @{
    DisplayName = "Block Legacy Authentication"
    State = "enabled"
    Conditions = @{ ClientAppTypes = @("exchangeActiveSync","other") }
    GrantControls = @{ Operator = "OR"; BuiltInControls = @("block") }
}
New-MgIdentityConditionalAccessPolicy -BodyParameter $policy

# Enable unified audit log
Set-AdminAuditLogConfig -UnifiedAuditLogIngestionEnabled $true
```

**Step 4: Provision Users**

```powershell
$licenseSku = (Get-MgSubscribedSku | Where-Object { $_.SkuPartNumber -eq "ENTERPRISEPACK" }).SkuId

Import-Csv .\employees.csv | ForEach-Object {
    try {
        $user = New-MgUser -DisplayName $_.DisplayName -UserPrincipalName $_.UserPrincipalName `
                           -AccountEnabled -PasswordProfile @{ Password = (New-Guid).ToString().Substring(0,12)+"!"; ForceChangePasswordNextSignIn = $true }
        Set-MgUserLicense -UserId $user.Id -AddLicenses @(@{ SkuId = $licenseSku }) -RemoveLicenses @()
        Write-Host "Provisioned: $($_.UserPrincipalName)"
    } catch {
        Write-Warning "Failed $($_.UserPrincipalName): $_"
    }
}
```

**Validation:** Spot-check 3–5 accounts in the M365 admin portal; confirm licenses show "Active."

---

### Workflow 2: Security Hardening

**Step 1: Run Security Audit**

```powershell
Connect-MgGraph -Scopes "Directory.Read.All","Policy.Read.All","AuditLog.Read.All","Reports.Read.All"

# Export Conditional Access policy inventory
Get-MgIdentityConditionalAccessPolicy | Select-Object DisplayName, State |
    Export-Csv .\ca_policies.csv -NoTypeInformation

# Find accounts without MFA registered
$report = Get-MgReportAuthenticationMethodUserRegistrationDetail
$report | Where-Object { -not $_.IsMfaRegistered } |
    Select-Object UserPrincipalName, IsMfaRegistered |
    Export-Csv .\no_mfa_users.csv -NoTypeInformation

Write-Host "Audit complete. Review ca_policies.csv and no_mfa_users.csv."
```

**Step 2: Create MFA Policy (report-only first)**

```powershell
$policy = @{
    DisplayName = "Require MFA All Users"
    State = "enabledForReportingButNotEnforced"
    Conditions = @{ Users = @{ IncludeUsers = @("All") } }
    GrantControls = @{ Operator = "OR"; BuiltInControls = @("mfa") }
}
New-MgIdentityConditionalAccessPolicy -BodyParameter $policy
```

**Validation:** After 48 h, review Sign-in logs in Entra ID; confirm expected users would be challenged, then change `State` to `"enabled"`.

**Step 3: Review Secure Score**

```powershell
# Retrieve current Secure Score and top improvement actions
Get-MgSecuritySecureScore -Top 1 | Select-Object CurrentScore, MaxScore, ActiveUserCount
Get-MgSecuritySecureScoreControlProfile | Sort-Object -Property ActionType |
    Select-Object Title, ImplementationStatus, MaxScore | Format-Table -AutoSize
```

---

### Workflow 3: User Offboarding

**Step 1: Block Sign-in and Revoke Sessions**

```powershell
$upn = "departing.user@company.com"
$user = Get-MgUser -Filter "userPrincipalName eq '$upn'"

# Block sign-in immediately
Update-MgUser -UserId $user.Id -AccountEnabled:$false

# Revoke all active tokens
Invoke-MgInvalidateAllUserRefreshToken -UserId $user.Id
Write-Host "Sign-in blocked and sessions revoked for $upn"
```

**Step 2: Preview with -WhatIf (license removal)**

```powershell
# Identify assigned licenses
$licenses = (Get-MgUserLicenseDetail -UserId $user.Id).SkuId

# Dry-run: print what would be removed
$licenses | ForEach-Object { Write-Host "[WhatIf] Would remove SKU: $_" }
```

**Step 3: Execute Offboarding**

```powershell
# Remove licenses
Set-MgUserLicense -UserId $user.Id -AddLicenses @() -RemoveLicenses $licenses

# Convert mailbox to shared (requires ExchangeOnlineManagement module)
Set-Mailbox -Identity $upn -Type Shared

# Remove from all groups
Get-MgUserMemberOf -UserId $user.Id | ForEach-Object {
    try { Remove-MgGroupMemberByRef -GroupId $_.Id -DirectoryObjectId $user.Id } catch {}
}
Write-Host "Offboarding complete for $upn"
```

**Validation:** Confirm in the M365 admin portal that the account shows "Blocked," has no active licenses, and the mailbox type is "Shared."

---

## Best Practices

### Tenant Setup

1. Enable MFA before adding users → verify: step output matches expected outcome
2. Configure named locations for Conditional Access → verify: step output matches expected outcome
3. Use separate admin accounts with PIM → verify: step output matches expected outcome
4. Verify custom domains (and DNS propagation) before bulk user creation
5. Apply Microsoft Secure Score recommendations → verify: diff matches intent

### Security Operations

1. Start Conditional Access policies in report-only mode → verify: step output matches expected outcome
2. Review Sign-in logs for 48 h before enforcing a new policy → verify: step output matches expected outcome
3. Never hardcode credentials in scripts — use Azure Key Vault or `Get-Credential` → verify: step output matches expected outcome
4. Enable unified audit logging for all operations → verify: findings count surfaced
5. Conduct quarterly security reviews and Secure Score check-ins → verify: all checks pass

### PowerShell Automation

1. Prefer Microsoft Graph (`Microsoft.Graph` module) over legacy MSOnline → verify: step output matches expected outcome
2. Include `try/catch` blocks for error handling → verify: step output matches expected outcome
3. Implement `Write-Host`/`Write-Warning` logging for audit trails → verify: output exists + parses without error
4. Use `-WhatIf` or dry-run output before bulk destructive operations → verify: command exit code 0
5. Test in a non-production tenant first → verify: all checks pass

---

## Reference Guides

**references/powershell-templates.md**
- Ready-to-use script templates
- Conditional Access policy examples
- Bulk user provisioning scripts
- Security audit scripts

**references/security-policies.md**
- Conditional Access configuration
- MFA enforcement strategies
- DLP and retention policies
- Security baseline settings

**references/troubleshooting.md**
- Common error resolutions
- PowerShell module issues
- Permission troubleshooting
- DNS propagation problems

---

## Limitations

| Constraint | Impact |
|------------|--------|
| Global Admin required | Full tenant setup needs highest privilege |
| API rate limits | Bulk operations may be throttled |
| License dependencies | E3/E5 required for advanced features |
| Hybrid scenarios | On-premises AD needs additional configuration |
| PowerShell prerequisites | Microsoft.Graph module required |

### Required PowerShell Modules

```powershell
Install-Module Microsoft.Graph -Scope CurrentUser
Install-Module ExchangeOnlineManagement -Scope CurrentUser
Install-Module MicrosoftTeams -Scope CurrentUser
```

### Required Permissions

- **Global Administrator** — Full tenant setup
- **User Administrator** — User management
- **Security Administrator** — Security policies
- **Exchange Administrator** — Mailbox management

## When NOT to use

- Task is unrelated to ms365 tenant manager — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Ms365 Tenant Manager needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for ms365 tenant manager
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving ms365 tenant manager
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

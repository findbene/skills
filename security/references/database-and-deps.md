# Database Security & Dependency Management

## SQL Injection Prevention

### Parameterized Queries (Always)
```python
# WRONG — string concatenation = SQL injection
user_input = "' OR '1'='1"
query = f"SELECT * FROM users WHERE email = '{user_input}'"

# RIGHT — parameterized
import psycopg2
cursor.execute(
    "SELECT * FROM users WHERE email = %s AND active = %s",
    (email, True)
)

# SQLAlchemy ORM (safe by default)
users = db.query(User).filter(User.email == email).all()

# SQLAlchemy raw SQL — use text() with named params
from sqlalchemy import text
result = db.execute(
    text("SELECT * FROM users WHERE email = :email"),
    {"email": email}
)
```

### Dynamic Column Names (Use Allowlists)
```python
ALLOWED_SORT_COLUMNS = {"created_at", "name", "email", "score"}
ALLOWED_DIRECTIONS = {"ASC", "DESC"}

def get_sorted(sort_by: str, direction: str):
    if sort_by not in ALLOWED_SORT_COLUMNS:
        raise ValueError(f"Invalid sort column: {sort_by}")
    if direction not in ALLOWED_DIRECTIONS:
        raise ValueError(f"Invalid direction: {direction}")
    # Safe to interpolate — validated against allowlist
    return db.execute(text(f"SELECT * FROM records ORDER BY {sort_by} {direction}"))
```

---

## Dependency Security

### Vulnerability Scanning
```bash
# Python
pip install pip-audit
pip-audit                      # scan current environment
pip-audit -r requirements.txt  # scan requirements file

# Node.js
npm audit
npm audit fix

# GitHub Dependabot — auto PRs for vulnerable deps
# .github/dependabot.yml:
```

```yaml
version: 2
updates:
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
    groups:
      security-patches:
        patterns: ["*"]
        update-types: ["security"]
```

### Pinning Dependencies (Supply Chain)
```
# requirements.txt — pin exact versions
anthropic==0.40.0
langchain==0.3.14

# Generate locked requirements with hashes
pip install pip-tools
pip-compile requirements.in   # generates requirements.txt
pip-sync requirements.txt     # installs exactly what's pinned
```

### Pre-commit Security Hooks
```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/PyCQA/bandit
    rev: 1.7.7
    hooks:
      - id: bandit
        args: ["-c", "pyproject.toml", "-r", ".", "--skip", "B101"]

  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ["--baseline", ".secrets.baseline"]
```

---

## Database Access Control (Supabase)

### Row Level Security
```sql
ALTER TABLE agent_outputs ENABLE ROW LEVEL SECURITY;

CREATE POLICY "isolation" ON agent_outputs
    FOR ALL
    USING (user_id = auth.uid());
```

### Least Privilege Database Users
```sql
CREATE USER app_user WITH PASSWORD 'strong-password';

GRANT SELECT, INSERT, UPDATE ON agent_outputs TO app_user;
GRANT SELECT ON content_plans TO app_user;

-- Read-only for analytics
CREATE USER analytics_user WITH PASSWORD 'another-password';
GRANT SELECT ON ALL TABLES IN SCHEMA public TO analytics_user;
```

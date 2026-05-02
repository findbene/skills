# Authentication & Secrets Management

## Secrets Management

### The Absolute Rules
```python
# WRONG — secret in source code
API_KEY = "sk-abc123"

# RIGHT — fail loudly if secret is missing
import os

def get_required_env(var: str) -> str:
    value = os.environ.get(var)
    if not value:
        raise RuntimeError(f"Required environment variable {var} is not set")
    return value

API_KEY = get_required_env("ANTHROPIC_API_KEY")
```

### .gitignore for Secrets
```gitignore
.env
.env.*
!.env.example
*.pem
*.key
credentials.json
service-account.json
*_rsa
*_ed25519
```

### Secret Scanning (Pre-commit)
```bash
pip install detect-secrets
detect-secrets scan > .secrets.baseline

# .pre-commit-config.yaml
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']
```

### HashiCorp Vault Integration
```python
import hvac

client = hvac.Client(url=os.environ["VAULT_ADDR"], token=os.environ["VAULT_TOKEN"])

def get_secret(path: str, key: str) -> str:
    response = client.secrets.kv.v2.read_secret_version(
        path=path, mount_point="secret"
    )
    return response["data"]["data"][key]
```

---

## JWT Security

### Strict Verification (Always)
```python
import jwt

def verify_token(token: str) -> dict:
    try:
        payload = jwt.decode(
            token,
            key=os.environ["JWT_SECRET"],
            algorithms=["HS256"],           # whitelist only — never include "none"
            options={
                "require": ["exp", "iat", "sub"],
                "verify_exp": True,
            }
        )
        return payload
    except jwt.ExpiredSignatureError:
        raise AuthError("Token has expired")
    except jwt.InvalidTokenError as e:
        raise AuthError(f"Invalid token: {e}")
```

### Token Best Practices
```python
from datetime import datetime, timezone, timedelta
import uuid

JWT_SECRET = get_required_env("JWT_SECRET")

def create_access_token(user_id: str) -> str:
    now = datetime.now(timezone.utc)
    payload = {
        "sub": user_id,
        "iat": now,
        "exp": now + timedelta(minutes=15),   # short-lived
        "type": "access",
        "jti": str(uuid.uuid4())              # unique ID (revocation support)
    }
    return jwt.encode(payload, JWT_SECRET, algorithm="HS256")
```

---

## OAuth 2.0 Security

### State Parameter (CSRF Protection)
```python
import secrets
import redis

r = redis.Redis()

def initiate_oauth_flow() -> str:
    state = secrets.token_urlsafe(32)
    r.setex(f"oauth_state:{state}", 600, "1")  # 10min TTL
    return state

def complete_oauth_flow(code: str, state: str) -> dict:
    if not r.get(f"oauth_state:{state}"):
        raise SecurityError("Invalid or expired OAuth state")
    r.delete(f"oauth_state:{state}")  # one-time use
    return exchange_code_for_tokens(code)
```

---

## Password Security

```python
import bcrypt

def hash_password(password: str) -> str:
    return bcrypt.hashpw(password.encode(), bcrypt.gensalt(rounds=12)).decode()

def verify_password(password: str, hashed: str) -> bool:
    return bcrypt.checkpw(password.encode(), hashed.encode())

# Preferred: argon2
from argon2 import PasswordHasher
ph = PasswordHasher(time_cost=3, memory_cost=65536, parallelism=4)
hashed = ph.hash(password)
ph.verify(hashed, password)  # raises VerifyMismatchError on failure
```

---

## Incident Response: Exposed Secret in Git

```bash
# Step 1: IMMEDIATELY rotate the secret at the provider (Anthropic, OpenAI, etc.)

# Step 2: Remove from git history
pip install git-filter-repo
git filter-repo --path .env --invert-paths
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# Step 3: Force push (required after history rewrite)
git push origin --force --all

# Step 4: Notify all collaborators to re-clone

# Step 5: Scan logs for unauthorized usage of old secret
```

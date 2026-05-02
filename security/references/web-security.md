# Web Application Security (OWASP Top 10)

## OWASP Top 10 Quick Reference

| # | Vulnerability | Key Prevention |
|---|---|---|
| A01 | Broken Access Control | Verify authorization on every request |
| A02 | Cryptographic Failures | TLS everywhere, bcrypt/argon2, encrypt at rest |
| A03 | Injection | Parameterized queries, input validation |
| A04 | Insecure Design | Threat modeling, secure design patterns |
| A05 | Security Misconfiguration | No default credentials, no verbose errors |
| A06 | Vulnerable Components | Dependency scanning, pin versions |
| A07 | Authentication Failures | MFA, brute-force protection |
| A08 | Software & Data Integrity | Code signing, secure CI/CD |
| A09 | Security Logging Failures | Log auth events, never log secrets |
| A10 | SSRF | Validate URLs, allowlist internal services |

---

## Security Headers (FastAPI)

```python
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def security_headers(request: Request, call_next):
    response = await call_next(request)
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["Strict-Transport-Security"] = "max-age=31536000; includeSubDomains"
    response.headers["Referrer-Policy"] = "strict-origin-when-cross-origin"
    response.headers["Content-Security-Policy"] = (
        "default-src 'self'; "
        "script-src 'self'; "
        "img-src 'self' data: https:; "
        "frame-ancestors 'none'"
    )
    return response

# CORS — restrictive
from fastapi.middleware.cors import CORSMiddleware
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://yourdomain.com"],  # never "*" for authenticated APIs
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE"],
    allow_headers=["Authorization", "Content-Type"],
)
```

---

## Input Validation (Pydantic)

```python
from pydantic import BaseModel, validator, Field
from typing import Literal
import re

class ScriptRequest(BaseModel):
    niche: str = Field(..., min_length=3, max_length=100)
    tier: Literal[1, 2, 3]
    duration_seconds: int = Field(..., ge=15, le=120)
    platform: Literal["youtube_shorts", "tiktok", "instagram_reels"]

    @validator("niche")
    def niche_must_be_safe(cls, v):
        if not re.match(r'^[a-zA-Z0-9 _-]+$', v):
            raise ValueError("Niche contains invalid characters")
        return v.strip()
```

---

## Rate Limiting (FastAPI)

```python
from slowapi import Limiter, _rate_limit_exceeded_handler
from slowapi.util import get_remote_address
from slowapi.errors import RateLimitExceeded

limiter = Limiter(key_func=get_remote_address, storage_uri=os.environ["REDIS_URL"])
app.state.limiter = limiter
app.add_exception_handler(RateLimitExceeded, _rate_limit_exceeded_handler)

@app.post("/api/scripts")
@limiter.limit("10/minute")
async def create_script(request: Request, body: ScriptRequest):
    ...

@app.post("/api/auth/login")
@limiter.limit("5/minute")
async def login(request: Request, credentials: LoginRequest):
    ...
```

---

## SSRF Prevention

```python
import ipaddress
from urllib.parse import urlparse

BLOCKED_HOSTS = {
    "localhost", "127.0.0.1", "0.0.0.0",
    "169.254.169.254",          # AWS metadata
    "metadata.google.internal", # GCP metadata
}

def validate_external_url(url: str) -> bool:
    try:
        parsed = urlparse(url)
        if parsed.scheme not in {"https"}:
            return False
        hostname = parsed.hostname
        if not hostname or hostname in BLOCKED_HOSTS:
            return False
        try:
            addr = ipaddress.ip_address(hostname)
            if addr.is_private or addr.is_loopback or addr.is_link_local:
                return False
        except ValueError:
            pass  # hostname, not IP
        return True
    except Exception:
        return False
```

---

## CSRF Protection

```python
from fastapi import Response, Cookie, Header
import secrets

def set_session_cookie(response: Response, session_id: str):
    response.set_cookie(
        key="session_id",
        value=session_id,
        httponly=True,
        secure=True,
        samesite="strict",   # prevents CSRF via cross-site requests
        max_age=3600,
    )

@app.post("/api/action")
async def protected_action(
    x_csrf_token: str = Header(...),
    csrf_cookie: str = Cookie(...)
):
    if not secrets.compare_digest(x_csrf_token, csrf_cookie):
        raise HTTPException(status_code=403, detail="CSRF validation failed")
```

---
name: auth-rbac-admin
description: "Secure authentication and RBAC implementation specialist covering JWT, OAuth2, session management, role hierarchies, and permission systems. Use this skill any time authentication needs to be implemented, role-based access control needs to be designed, OAuth or JWT needs to be configured, or user permission systems need to be structured. Trigger immediately on: \"authentication\", \"auth\", \"RBAC\", \"role-based access\", \"JWT\", \"OAuth\", \"session management\", \"permissions\", \"login\", \"authorization\", \"SSO\", \"user roles\", \"access control\", \"API keys\". If someone says \"implement login\" or \"restrict access to this endpoint\" this skill MUST trigger."
---

# Authentication & RBAC Administration

Guide the design and implementation of robust authentication and authorization systems.

## Core Authentication Patterns

### Session-Based Authentication

```typescript
const sessionConfig = {
  secret: process.env.SESSION_SECRET,
  name: '__session',
  resave: false,
  saveUninitialized: false,
  cookie: {
    httpOnly: true,
    secure: process.env.NODE_ENV === 'production',
    sameSite: 'strict',
    maxAge: 24 * 60 * 60 * 1000
  }
};
```

### JWT Authentication

Short-lived access tokens (15 min) paired with long-lived refresh tokens (7-30 days) balance security with user experience. Short access tokens limit the damage window if a token is compromised.

```typescript
interface JWTPayload {
  sub: string;
  iat: number;
  exp: number;
  roles: string[];
  permissions: string[];
}

function generateToken(user: User): string {
  return jwt.sign(
    { sub: user.id, roles: user.roles, permissions: user.permissions },
    process.env.JWT_SECRET,
    { expiresIn: '15m' }
  );
}
```

Rotate refresh tokens on use and store them securely (HTTP-only cookies or encrypted storage).

### OAuth 2.0 / OpenID Connect

Authorization Code Flow with PKCE for public clients:

```typescript
const oauthConfig = {
  authorization_endpoint: 'https://auth.example.com/authorize',
  token_endpoint: 'https://auth.example.com/token',
  client_id: process.env.OAUTH_CLIENT_ID,
  client_secret: process.env.OAUTH_CLIENT_SECRET,
  redirect_uri: 'https://app.example.com/callback',
  scope: 'openid profile email',
  response_type: 'code',
  code_challenge_method: 'S256'
};
```

## Role-Based Access Control (RBAC)

### Role Hierarchy

Hierarchical roles reduce duplication — each role inherits permissions from its parent, so you only define the delta.

```typescript
const roles = {
  admin: {
    inherits: ['manager'],
    permissions: ['users:delete', 'settings:manage', 'audit:view']
  },
  manager: {
    inherits: ['user'],
    permissions: ['users:create', 'users:update', 'reports:generate']
  },
  user: {
    inherits: [],
    permissions: ['profile:read', 'profile:update', 'content:read']
  }
};
```

### Permission Checking

```typescript
function requirePermission(permission: string) {
  return (req: Request, res: Response, next: NextFunction) => {
    if (!req.user) return res.status(401).json({ error: 'Unauthorized' });
    if (!hasPermission(req.user, permission)) return res.status(403).json({ error: 'Forbidden' });
    next();
  };
}

function hasPermission(user: User, permission: string): boolean {
  const userPermissions = getAllPermissions(user.roles);
  return userPermissions.includes(permission);
}
```

### Attribute-Based Access Control (ABAC)

For fine-grained access beyond roles — when authorization depends on resource attributes, user attributes, or environmental conditions:

```typescript
interface Policy {
  effect: 'allow' | 'deny';
  action: string;
  resource: string;
  condition?: (context: Context) => boolean;
}

const policies: Policy[] = [
  {
    effect: 'allow',
    action: 'document:read',
    resource: 'document/*',
    condition: (ctx) => ctx.resource.ownerId === ctx.subject.id
  }
];
```

## Security Best Practices

### Password Security
Use bcrypt with 12+ salt rounds. Enforce minimum 12 characters with mixed case, numbers, and special characters. Prevent reuse of last 5 passwords.

### Multi-Factor Authentication (MFA)
```typescript
import { authenticator } from 'otplib';

function generateSecret(): string { return authenticator.generateSecret(); }
function verifyTOTP(token: string, secret: string): boolean {
  return authenticator.verify({ token, secret });
}
```

### Rate Limiting & Account Lockout
Lock accounts after 5 failed attempts for 15 minutes. This balances security (preventing brute force) with usability (legitimate users who mistype).

### Audit Logging
Log all authentication events (login, logout, failed attempts, password changes, MFA enrollment) with timestamp, user ID, IP address, and user agent. Audit logs are essential for incident response and compliance.

## Critical Security Rules

1. Store passwords only as bcrypt/argon2 hashes
2. Use HTTPS for all authentication endpoints
3. Implement proper session invalidation on logout
4. Rotate secrets and tokens regularly
5. Log all authentication events
6. Use secure cookie settings (HttpOnly, Secure, SameSite)
7. Validate and sanitize all input

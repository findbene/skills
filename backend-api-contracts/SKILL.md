---
name: backend-api-contracts
description: 'Production-ready backend API implementation expert covering OpenAPI contracts, request validation, error handling, middl. Triggers: "use backend-api-contracts", "backend api contracts", "backend task.'
allowed-tools: Glob, Grep, Read
---

# Backend API Contracts & Development

Build production-ready backend services with proper API design, data validation, error handling, and documentation.

## RESTful API Standards

```
GET    /api/v1/users           # List users
POST   /api/v1/users           # Create user
GET    /api/v1/users/:id       # Get user
PUT    /api/v1/users/:id       # Update user (full)
PATCH  /api/v1/users/:id       # Update user (partial)
DELETE /api/v1/users/:id       # Delete user
GET    /api/v1/users/:id/orders # Nested resources
```

## Repository Pattern

Separating data access from business logic makes code testable and swappable between databases:

```typescript
interface Repository<T, ID> {
  findById(id: ID): Promise<T | null>;
  findAll(options?: FindOptions): Promise<T[]>;
  create(data: Partial<T>): Promise<T>;
  update(id: ID, data: Partial<T>): Promise<T>;
  delete(id: ID): Promise<void>;
}
```

## Service Layer

Business logic lives here — validation rules, side effects, and orchestration:

```typescript
class UserService {
  constructor(
    private userRepo: UserRepository,
    private emailService: EmailService,
    private eventBus: EventBus
  ) {}

  async createUser(data: CreateUserDTO): Promise<User> {
    const existingUser = await this.userRepo.findByEmail(data.email);
    if (existingUser) throw new ConflictError('Email already registered');

    const hashedPassword = await hashPassword(data.password);
    const user = await this.userRepo.create({ ...data, password: hashedPassword });

    await this.emailService.sendWelcome(user.email);
    await this.eventBus.publish('user.created', { userId: user.id });
    return user;
  }
}
```

## Data Validation with Zod

Validate at API boundaries — this is the wall between untrusted user input and your application logic:

```typescript
import { z } from 'zod';

const CreateUserSchema = z.object({
  email: z.string().email(),
  password: z.string().min(12).regex(/[A-Z]/).regex(/[0-9]/),
  name: z.string().min(1).max(100),
  role: z.enum(['user', 'admin']).default('user')
});

function validate<T>(schema: z.ZodSchema<T>) {
  return (req: Request, res: Response, next: NextFunction) => {
    const result = schema.safeParse(req.body);
    if (!result.success) {
      return res.status(400).json({ error: 'Validation failed', details: result.error.errors });
    }
    req.validatedBody = result.data;
    next();
  };
}
```

## Standardized Error Handling

Consistent error formats let API consumers write reliable error handling:

```typescript
class AppError extends Error {
  constructor(
    public statusCode: number,
    public code: string,
    message: string,
    public details?: any
  ) { super(message); }
}

class ValidationError extends AppError {
  constructor(details: any) { super(400, 'VALIDATION_ERROR', 'Validation failed', details); }
}

class NotFoundError extends AppError {
  constructor(resource: string) { super(404, 'NOT_FOUND', `${resource} not found`); }
}

function errorHandler(err: Error, req: Request, res: Response, next: NextFunction) {
  const requestId = req.headers['x-request-id'];
  if (err instanceof AppError) {
    return res.status(err.statusCode).json({
      error: { code: err.code, message: err.message, details: err.details, requestId, timestamp: new Date().toISOString() }
    });
  }
  logger.error({ err, requestId }, 'Unhandled error');
  res.status(500).json({
    error: { code: 'INTERNAL_ERROR', message: 'An unexpected error occurred', requestId, timestamp: new Date().toISOString() }
  });
}
```

## Database Patterns

### Connection Pooling
```typescript
const pool = new Pool({
  host: process.env.DB_HOST,
  port: parseInt(process.env.DB_PORT || '5432'),
  database: process.env.DB_NAME,
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000
});
```

### Transactions
```typescript
async function transferFunds(fromId: string, toId: string, amount: number) {
  const client = await pool.connect();
  try {
    await client.query('BEGIN');
    await client.query('UPDATE accounts SET balance = balance - $1 WHERE id = $2', [amount, fromId]);
    await client.query('UPDATE accounts SET balance = balance + $1 WHERE id = $2', [amount, toId]);
    await client.query('COMMIT');
  } catch (e) {
    await client.query('ROLLBACK');
    throw e;
  } finally {
    client.release();
  }
}
```

## Best Practices

1. **Version your APIs** — Use URL or header versioning → verify: step output matches expected outcome
2. **Validate all input** — Untrusted data is the #1 attack vector → verify: all checks pass
3. **Use proper status codes** — Be specific about errors → verify: step output matches expected outcome
4. **Implement pagination** — Unbounded queries will eventually crash your server → verify: step output matches expected outcome
5. **Document with OpenAPI** — Keep docs in sync with code → verify: file content matches expected shape
6. **Use connection pooling** — Individual connections don't scale → verify: step output matches expected outcome
7. **Implement rate limiting** — Protect against abuse → verify: step output matches expected outcome
8. **Add request tracing** — Include request IDs for debugging → verify: dependency resolves + import works

## When NOT to use

- Task is unrelated to backend api contracts — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Backend Api Contracts needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for backend api contracts
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving backend api contracts
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

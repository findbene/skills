---
name: backend-api-contracts
description: "Production-ready backend API implementation expert covering OpenAPI contracts, request validation, error handling, middleware patterns, and API testing. Use this skill any time backend API endpoints need to be implemented, API contracts need to be enforced, request/response validation needs to be added, or API middleware needs to be written. Trigger immediately on: \"backend API\", \"API endpoint\", \"request validation\", \"OpenAPI\", \"API middleware\", \"error handler\", \"response schema\", \"API contract\", \"Pydantic model\", \"FastAPI\", \"Express route\", \"API testing\", \"endpoint validation\", \"API implementation\". If someone says \"implement this API endpoint\" or \"add validation to my API\" this skill MUST trigger."
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

1. **Version your APIs** — Use URL or header versioning
2. **Validate all input** — Untrusted data is the #1 attack vector
3. **Use proper status codes** — Be specific about errors
4. **Implement pagination** — Unbounded queries will eventually crash your server
5. **Document with OpenAPI** — Keep docs in sync with code
6. **Use connection pooling** — Individual connections don't scale
7. **Implement rate limiting** — Protect against abuse
8. **Add request tracing** — Include request IDs for debugging

---
name: typescript-strict
description: "Strict TypeScript patterns including generics, utility types, discriminated unions, Zod validation, and type safety enforcement across full-stack TypeScript. Use this skill any time TypeScript code needs to be written with strict type safety, generics need to be used, type errors need to be resolved, or TypeScript needs to be configured for strictness. Trigger immediately on: \"TypeScript\", \"type safety\", \"generics\", \"Zod\", \"type guard\", \"discriminated union\", \"utility types\", \"tsconfig\", \"strict mode\", \"any type\", \"type error\", \"infer\", \"conditional types\", \"mapped types\". If someone says \"fix my TypeScript errors\" or \"add type safety to this\" this skill MUST trigger."
---

# Strict TypeScript Patterns

Best practices for type-safe TypeScript code.

## Strict Configuration

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "exactOptionalPropertyTypes": true
  }
}
```

## Type Narrowing

```typescript
// Type guards
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

// Discriminated unions
type Result<T> =
  | { success: true; data: T }
  | { success: false; error: Error };

function handleResult<T>(result: Result<T>) {
  if (result.success) {
    console.log(result.data); // T
  } else {
    console.log(result.error); // Error
  }
}
```

## Utility Types

```typescript
type PartialUser = Partial<User>;
type RequiredUser = Required<User>;
type UserName = Pick<User, 'firstName' | 'lastName'>;
type UserWithoutId = Omit<User, 'id'>;
type UserRoles = Record<string, Role>;
type OnlyStrings = Extract<string | number | boolean, string>;
```

## Generic Patterns

```typescript
// Constrained generics
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

// Infer in conditional types
type Awaited<T> = T extends Promise<infer U> ? U : T;
```

## Avoid These Patterns

```typescript
// BAD: any → GOOD: unknown with narrowing
function process(data: unknown) {
  if (typeof data === 'string') { /* data is string */ }
}

// BAD: Non-null assertion → GOOD: Null check
const element = document.getElementById('app');
if (!element) throw new Error('Element not found');

// BAD: Type assertion → GOOD: Type guard or validation
const user = validateUser(data);
```

## Zod for Runtime Validation

```typescript
import { z } from 'zod';

const UserSchema = z.object({
  id: z.string().uuid(),
  email: z.string().email(),
  age: z.number().min(0).optional()
});

type User = z.infer<typeof UserSchema>;

const result = UserSchema.safeParse(data);
if (result.success) {
  const user: User = result.data;
}
```

## Error Handling

```typescript
class ApiError extends Error {
  constructor(message: string, public statusCode: number, public code: string) {
    super(message);
    this.name = 'ApiError';
  }
}

type Result<T, E = Error> =
  | { ok: true; value: T }
  | { ok: false; error: E };
```

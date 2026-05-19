---
name: typescript-strict
description: 'Strict TypeScript patterns including generics, utility types, discriminated unions, Zod validation, and type safety enforceme. Triggers: "use typescript-strict", "typescript strict", "typescript task.'
allowed-tools: Glob, Grep, Read
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

## When NOT to use

- Task is unrelated to typescript strict — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Typescript Strict needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for typescript strict
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving typescript strict
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

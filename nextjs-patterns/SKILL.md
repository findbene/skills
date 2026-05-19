---
name: nextjs-patterns
description: 'Next.js 14+ App Router specialist for Server Components, Server Actions, route handlers, middleware, caching, and loading/error bound. Triggers: "use nextjs-patterns", "nextjs patterns", "nextjs task.'
allowed-tools: Glob, Grep, Read
---

# Next.js App Router Patterns

Modern patterns for Next.js 14+ with App Router.

## Server Components (Default)

```typescript
// app/page.tsx - Server Component by default
async function Page() {
  const data = await fetchData(); // Direct async/await
  return <div>{data.title}</div>;
}
```

## Client Components

```typescript
'use client';
import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

## Server Actions

```typescript
'use server';
import { revalidatePath } from 'next/cache';

export async function createItem(formData: FormData) {
  const title = formData.get('title');
  await db.items.create({ title });
  revalidatePath('/items');
}
```

## Data Fetching Patterns

```typescript
// Parallel fetching
const [users, posts] = await Promise.all([fetchUsers(), fetchPosts()]);

// With caching
import { unstable_cache } from 'next/cache';
const getCachedUser = unstable_cache(
  async (id: string) => db.users.findUnique({ where: { id } }),
  ['user'],
  { revalidate: 3600, tags: ['user'] }
);

// Revalidation
import { revalidateTag, revalidatePath } from 'next/cache';
revalidateTag('user');
revalidatePath('/dashboard');
```

## Route Handlers

```typescript
// app/api/items/route.ts
import { NextRequest, NextResponse } from 'next/server';

export async function GET(request: NextRequest) {
  const items = await db.items.findMany();
  return NextResponse.json(items);
}

export async function POST(request: NextRequest) {
  const body = await request.json();
  const item = await db.items.create({ data: body });
  return NextResponse.json(item, { status: 201 });
}
```

## Middleware

```typescript
// middleware.ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
  const token = request.cookies.get('token');
  if (!token && request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.redirect(new URL('/login', request.url));
  }
  return NextResponse.next();
}

export const config = { matcher: ['/dashboard/:path*', '/api/:path*'] };
```

## Loading & Error States

```typescript
// app/dashboard/loading.tsx
export default function Loading() { return <Skeleton />; }

// app/dashboard/error.tsx
'use client';
export default function Error({ error, reset }) {
  return (
    <div>
      <p>Something went wrong</p>
      <button onClick={reset}>Try again</button>
    </div>
  );
}
```

## File Conventions

| File | Purpose |
|------|---------|
| `page.tsx` | Route UI |
| `layout.tsx` | Shared layout |
| `loading.tsx` | Loading UI |
| `error.tsx` | Error UI |
| `not-found.tsx` | 404 UI |
| `route.ts` | API endpoint |
| `template.tsx` | Re-render on navigation |

## When NOT to use

- Task is unrelated to nextjs patterns — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Nextjs Patterns needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for nextjs patterns
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving nextjs patterns
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

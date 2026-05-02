---
name: nextjs-patterns
description: "Next.js 14+ App Router specialist for Server Components, Server Actions, route handlers, middleware, caching, and loading/error boundaries. Use this skill any time Next.js App Router code is being written or reviewed, Server Components need to be implemented, Server Actions are being built, or caching and revalidation needs to be configured. Trigger immediately on: \"Next.js\", \"App Router\", \"server component\", \"server action\", \"revalidate\", \"middleware\", \"page.tsx\", \"layout.tsx\", \"loading.tsx\", \"use client\", \"use server\", \"next/cache\", \"route handler\", \"app directory\". If someone is building with Next.js or working in an app/ directory this skill MUST trigger."
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

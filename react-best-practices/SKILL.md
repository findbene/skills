---
name: react-best-practices
description: 'React and Next.js performance optimization from Vercel Engineering guidelines covering rendering patterns, memo, lazy loadin. Triggers: "use react-best-practices", "react best practices", "react task.'
allowed-tools: Glob, Grep, Read
---

# React Best Practices (Vercel)

Performance optimization rules from Vercel Engineering — 57 rules across 8 priority categories.

## Priority Categories

| Priority | Category | Severity | Focus |
|----------|----------|----------|-------|
| 1 | Waterfalls | CRITICAL | Eliminate async waterfalls |
| 2 | Bundle Size | CRITICAL | Reduce JS payload |
| 3 | Server Performance | HIGH | Optimize SSR/RSC |
| 4 | Client Data | MEDIUM-HIGH | Efficient client fetching |
| 5 | Re-renders | MEDIUM | Prevent unnecessary renders |
| 6 | Rendering | MEDIUM | Optimize paint/layout |
| 7 | JavaScript | LOW-MEDIUM | JS micro-optimizations |
| 8 | Advanced | LOW | Edge case patterns |

## Critical Rules

### Waterfall Prevention
```typescript
// BAD: Sequential waterfalls
const user = await getUser();
const posts = await getPosts(user.id);

// GOOD: Parallel fetching
const [user, posts] = await Promise.all([getUser(), getPosts(userId)]);
```

### Bundle Size
```typescript
// BAD: Barrel imports
import { Button, Card, Modal } from '@/components';

// GOOD: Direct imports
import { Button } from '@/components/Button';

// GOOD: Dynamic imports for heavy components
const Chart = dynamic(() => import('@/components/Chart'), { loading: () => <Skeleton /> });
```

### Server Optimization
```typescript
const getCachedData = unstable_cache(
  async (id) => fetchData(id),
  ['data-cache'],
  { revalidate: 3600 }
);
```

### Re-render Prevention
```typescript
// BAD: Inline objects cause re-renders
<Component style={{ margin: 10 }} />

// GOOD: Stable references
const style = useMemo(() => ({ margin: 10 }), []);
<Component style={style} />

// Defer expensive reads
const [isPending, startTransition] = useTransition();
startTransition(() => setFilter(value));
```

## Key Rules Checklist

- No sequential awaits (use Promise.all)
- No barrel file imports
- Dynamic import heavy components
- Use React.memo for expensive components
- Stable object/array references in props
- useTransition for non-urgent updates
- Cache server-side data fetching
- Proper Suspense boundaries

## When NOT to use

- Task is unrelated to react best practices — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | React Best Practices needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for react best practices
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving react best practices
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

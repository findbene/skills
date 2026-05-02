---
name: react-best-practices
description: "React and Next.js performance optimization from Vercel Engineering guidelines covering rendering patterns, memo, lazy loading, Suspense, and component architecture. Use this skill any time React performance needs to be optimized, React best practices need to be applied, unnecessary re-renders need to be eliminated, or React patterns need to be reviewed. Trigger immediately on: \"React performance\", \"re-render\", \"useMemo\", \"useCallback\", \"React.memo\", \"Suspense\", \"lazy loading React\", \"React optimization\", \"React profiler\", \"component performance\", \"React best practices\", \"React patterns\", \"key prop\", \"virtual DOM\". If someone says \"my React component is slow\" or \"optimize this React code\" this skill MUST trigger."
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

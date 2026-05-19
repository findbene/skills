---
name: code-review
description: 'Systematic code review methodology for PR reviews, implementation feedback, security checks, and code quality assessment. Triggers: "use code-review", "code review", "code task".'
allowed-tools: Glob, Grep, Read
---

# Code Review

Systematic methodology for thorough code reviews.

## Review Categories

### 1. Correctness
- Does it do what it's supposed to do?
- Are edge cases handled?
- Are error states managed?

### 2. Security
- Input validation present?
- SQL injection prevention?
- XSS protection?
- Authentication/authorization checks?
- Secrets not hardcoded?

### 3. Performance
- N+1 queries?
- Unnecessary re-renders?
- Missing indexes?
- Large bundle imports?
- Memory leaks?

### 4. Maintainability
- Clear naming?
- Single responsibility?
- DRY (Don't Repeat Yourself)?
- Appropriate abstraction level?
- Tests included?

## Review Checklist

```markdown
## Functionality
- [ ] Meets requirements
- [ ] Edge cases handled
- [ ] Error handling appropriate

## Code Quality
- [ ] Clear naming conventions
- [ ] No dead code
- [ ] No console.logs/debuggers
- [ ] No hardcoded values

## Security
- [ ] Input validated
- [ ] No SQL injection risks
- [ ] No XSS vulnerabilities
- [ ] Auth checks in place
- [ ] No exposed secrets

## Performance
- [ ] No N+1 queries
- [ ] Efficient algorithms
- [ ] Appropriate caching

## Testing
- [ ] Tests added/updated
- [ ] Tests pass
- [ ] Edge cases covered
```

## Common Red Flags

```typescript
// SQL injection
const query = `SELECT * FROM users WHERE id = ${userId}`;

// XSS vulnerability
element.innerHTML = userInput;

// Hardcoded secret
const API_KEY = 'sk-123456';

// N+1 query
users.forEach(async (user) => {
  const posts = await db.posts.findMany({ where: { userId: user.id } });
});

// Barrel import (tree-shaking killer)
import { Button } from '@/components';
```

## Feedback Patterns

Use comment prefixes to signal intent:
- `nit:` — Minor style issue, optional
- `suggestion:` — Improvement idea
- `question:` — Seeking clarification
- `issue:` — Must be addressed
- `praise:` — Positive feedback

Frame feedback constructively: "Consider using X because Y" rather than "This is wrong."

## When NOT to use

- Task is unrelated to code review — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Code Review needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for code review
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving code review
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

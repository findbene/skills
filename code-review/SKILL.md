---
name: code-review
description: "Systematic code review methodology for PR reviews, implementation feedback, security checks, and code quality assessment. Use this skill any time code needs to be reviewed, a PR needs feedback, implementation quality needs to be evaluated, or code needs to be checked before merging. Trigger immediately on: \"review this code\", \"PR review\", \"code feedback\", \"check my code\", \"review my changes\", \"code review\", \"review this PR\", \"is this good code\", \"look at my implementation\", \"review my commit\", \"audit this file\", \"code quality check\", \"what do you think of this code\". If someone shares code and says \"take a look\" or \"review this\" this skill MUST trigger."
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

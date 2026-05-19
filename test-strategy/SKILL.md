---
name: test-strategy
description: 'Comprehensive testing strategy design covering unit, integration, E2E, contract, and performance testing frameworks and methodologies. Triggers: "use test-strategy", "test strategy", "test task".'
allowed-tools: Glob, Grep, Read
---

# Test Strategy

Guide for comprehensive testing strategies and implementation.

## Testing Pyramid

```
        /\
       /  \     E2E Tests (few, 10%)
      /----\
     /      \   Integration Tests (some, 20%)
    /--------\
   /          \ Unit Tests (many, 70%)
  /------------\
```

## Test Types

### Unit Tests (Jest, Vitest)
```javascript
describe('calculateTotal', () => {
  it('should sum items correctly', () => {
    const items = [{ price: 10 }, { price: 20 }];
    expect(calculateTotal(items)).toBe(30);
  });

  it('should handle empty array', () => {
    expect(calculateTotal([])).toBe(0);
  });
});
```

### Component Tests (React Testing Library)
```javascript
test('renders button and handles click', () => {
  const handleClick = jest.fn();
  render(<Button onClick={handleClick}>Click me</Button>);
  fireEvent.click(screen.getByText('Click me'));
  expect(handleClick).toHaveBeenCalledTimes(1);
});
```

### API Tests
```javascript
describe('POST /api/users', () => {
  it('creates user with valid data', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com' });
    expect(response.status).toBe(201);
  });
});
```

### E2E Tests (Playwright)
```javascript
test('user can complete checkout', async ({ page }) => {
  await page.goto('/products');
  await page.click('[data-testid="add-to-cart"]');
  await page.click('[data-testid="checkout"]');
  await page.fill('#email', 'test@example.com');
  await page.click('[data-testid="submit"]');
  await expect(page.locator('.confirmation')).toBeVisible();
});
```

## Best Practices

- **AAA Pattern**: Arrange, Act, Assert
- **One assertion per test** (when practical)
- **Descriptive test names**: Describe behavior, not implementation
- **Test behavior, not implementation**
- **Keep tests independent**: No shared state
- **Use fixtures and factories** for test data
- **CI/CD integration**: Run tests on every commit

## When NOT to use

- Task is unrelated to test strategy — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Test Strategy needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for test strategy
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving test strategy
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

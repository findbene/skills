---
name: test-strategy
description: "Comprehensive testing strategy design covering unit, integration, E2E, contract, and performance testing frameworks and methodologies. Use this skill any time a testing strategy needs to be designed, test coverage needs to be planned, testing frameworks need to be selected, or a QA approach needs to be established. Trigger immediately on: \"test strategy\", \"testing plan\", \"unit test\", \"integration test\", \"E2E test\", \"test coverage\", \"testing framework\", \"TDD\", \"BDD\", \"Jest\", \"Pytest\", \"test pyramid\", \"test architecture\", \"QA strategy\". If someone says \"how should we test this?\" or \"design our testing approach\" this skill MUST trigger."
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

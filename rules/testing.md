# Testing Rules

## Test Philosophy

- Tests are documentation
- Test behavior, not implementation
- One concept per test
- Fast, isolated, repeatable

## Test Structure

```
describe('Feature', () => {
  describe('when condition', () => {
    it('should behave like this', () => {
      // Arrange
      // Act
      // Assert
    });
  });
});
```

## Coverage Requirements

- Minimum 80% code coverage
- 100% coverage for critical paths
- All public APIs must have tests

## Test Types

1. **Unit tests**: Fast, isolated, test single functions
2. **Integration tests**: Test component interactions
3. **E2E tests**: Test user workflows (keep minimal)

## Best Practices

- Use descriptive test names
- Avoid test interdependencies
- Mock external dependencies
- Test edge cases and error conditions

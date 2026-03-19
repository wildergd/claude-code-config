## Test Types (Required)

### 1. Unit Tests
- Functions → pure logic and edge cases  
- Hooks → state transitions and side effects in isolation  
- Components → behavior via user interactions (not internals)  

### 2. Integration Tests
- Hooks → state transitions and side effects in isolation (mock external dependencies)
- Multi-component user flows  
- API boundary contracts  

## What to Test

- Business logic and transformations  
- State changes and side effects  
- User-visible behavior and interactions  
- Error states and edge cases  

## What NOT to Test

- Implementation details (internal state, private functions)  
- Styling, layout, or non-functional UI  
- Third-party library internals  
- Code with no branching logic or state (no meaningful assertions possible)  

## Test Quality Standards

- Tests must be deterministic (no flaky behavior)  
- Tests must be isolated (no shared state or cross-test dependencies)  
- Prefer readability over cleverness  
- Mock only external dependencies (APIs, services), not internal logic 
- Use AAA pattern with section comments (# Arrange / # Act / # Assert)
- Flag coverage gaps rather than padding with low-value tests 

## Troubleshooting Test Failures

Fix the implementation, not the tests — unless the test itself is  
asserting the wrong behavior. Use tdd-guide for complex cases.  

## Coverage

Enforce **90% coverage** via Jest/Vitest (`coverageThreshold`).  

Treat it as a floor, not a target:  
- Missing coverage on critical paths matters more than the aggregate number  
- Do not add low-value tests solely to increase coverage  
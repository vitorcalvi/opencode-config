---
name: test-generator
description: Automated test generation skill that creates comprehensive test suites for codebases.
---

# Test Generator Skill

## Purpose
Automated test generation skill that creates comprehensive test suites for codebases.

## Capabilities
1. **Unit Test Generation**
   - Test cases for individual functions
   - Edge case identification
   - Mock object creation
   - Test data generation

2. **Integration Test Creation**
   - API endpoint testing
   - Database integration tests
   - Service interaction tests
   - Third-party integration validation

3. **E2E Test Generation**
   - User journey mapping
   - Cross-feature workflow tests
   - UI automation scripts
   - End-to-end scenario coverage

4. **Test Coverage Analysis**
   - Coverage percentage calculation
   - Uncovered code identification
   - Critical path analysis
   - Coverage improvement suggestions

5. **Test Pattern Recognition**
   - Given-When-Then (Gherkin) scenarios
   - Arrange-Act-Assert (AAA) patterns
   - Data-driven test generation
   - Property-based testing setup

## Usage

### Generate Unit Tests
```
Generate comprehensive unit tests for the user authentication service
```

### Create Integration Tests
```
Create integration tests for the payment processing API
```

### Generate E2E Tests
```
Generate end-to-end tests for the user registration flow
```

### With Specific Coverage
```
Generate tests with:
- Minimum 90% code coverage
- Focus on critical business logic paths
- Include edge cases and error scenarios
```

## Testing Frameworks
Supports:
- JavaScript/TypeScript: Jest, Vitest, Mocha, Cypress, Playwright
- Python: pytest, unittest, Robot Framework
- Go: testing package, testify
- Rust: cargo test
- Java: JUnit, TestNG

## Best Practices
1. Test behavior, not implementation
2. Use descriptive test names
3. Arrange-Act-Assert (AAA) pattern
4. Test both happy and sad paths
5. Mock external dependencies
6. Keep tests independent
7. Use test doubles appropriately

## Output
- **Test Suite**: Complete test files
- **Test Data**: Mock and fixture data
- **Coverage Report**: Code coverage analysis
- **Test Documentation**: Explanation of test scenarios
- **Run Instructions**: How to execute generated tests

## Integration
Works with:
- Test runners and frameworks
- Coverage tools
- Mock/stub libraries
- CI/CD pipeline integration

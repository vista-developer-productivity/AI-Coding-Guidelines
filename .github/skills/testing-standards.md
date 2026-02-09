# Testing Standards

## General Testing Rules

- Write unit tests for all functions and classes, covering critical paths and edge cases
- Aim for **80%+ code coverage**, **95%+ for critical business logic**
- Use descriptive test names that explain the scenario and expected outcome
- Follow **AAA pattern**: Arrange, Act, Assert
- Mock external dependencies and side effects
- Test error conditions and edge cases explicitly

## Test Framework Selection (per Tech Radars)

| Language | Unit Testing | E2E Testing |
|----------|-------------|-------------|
| TypeScript/JavaScript | **Vitest** (preferred), Jest, Testing Library | **Playwright** (adopted) |
| Go | `testing/go`, testify assert/require | N/A |
| Python | pytest | Playwright |
| C#/.NET | xUnit, Moq/NSubstitute | Playwright |
| Java | JUnit 5 | Playwright |

> ⚠️ **Cypress and Selenium are Hold**, use Playwright for all new e2e testing (per Frontend Tech Radar RFC, Jan 2026)

## Test Types

- **Unit Tests**: Single component in isolation. Mock driven ports, fake repositories.
- **Integration Tests**: Real I/O boundaries (database, API, filesystem, time).
- **E2E Tests**: Full user flows via Playwright. Use `@playwright/test` with locator syntax.
- **Contract Tests**: HTTP/API contracts only.

## Test Quality Checklist

- [ ] Tests are isolated and deterministic (no shared mutable state)
- [ ] Tests are fast (unit tests < 100ms each)
- [ ] Test names describe the scenario and expected outcome
- [ ] External dependencies are mocked/stubbed
- [ ] Error paths and edge cases are tested
- [ ] No hardcoded test data that could become stale
- [ ] Tests serve as documentation of expected behavior

## Mock Discipline

- ✅ Mock driven ports (external dependencies)
- ✅ Use fake/in-memory implementations for repositories in acceptance tests
- ❌ Never mock domain objects (entities, value objects, aggregates)
- ❌ Never verify queries (only commands with side effects)

## Playwright E2E Best Practices

- Use locator syntax (`page.getByRole()`, `page.getByText()`), mirrors Testing Library
- Use `async/await` for all interactions
- Run tests in parallel where possible
- Use `testInfo.outputDir` for artifacts
- Implement Page Object Model for complex flows

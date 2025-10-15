---
description: 'Instructions for writing Go tests following idiomatic practices and community standards'
applyTo: '**/*_test.go'
---

# Go Testing Instructions [v1.0]

Follow idiomatic Go testing practices and community standards when writing tests. These instructions are based on the [Go testing package documentation](https://pkg.go.dev/testing), [testify assertions library](https://github.com/stretchr/testify), and community best practices. These instructions complement the general Go development guidelines and emphasize clarity, maintainability, and practical testing approaches.

## Philosophy & Approach

- Automated testing is a first-class aspect of Go development
- Favor **clarity, simplicity, and behavior testing** over overengineering
- Avoid heavy abstractions or mocks unless absolutely necessary
- When mocking is needed, prefer small hand-rolled fakes or stubs over complex generated mocks unless the dependency is complex
- Strive for tests that are self-contained, deterministic, and maintainable
- Tests should serve as documentation and examples of how code should be used

## Test Organization and Structure

### File and Function Conventions

- **Test colocation**: Tests must live in the same package (or the `_test` variant) as the code they test, with a filename ending in `_test.go`
- Test function names should start with **`TestXxx`**, benchmark functions `BenchmarkXxx`, fuzz tests `FuzzXxx`
- Place test files next to the code they test
- Keep tests in the same package for white-box testing
- Use `_test` package suffix for black-box testing when appropriate

### Test Structure Patterns

- Use **table-driven tests** with `t.Run(...)` for enumerating multiple scenarios
- For parallelizable cases, use `t.Parallel()` in subtests (or at top of `TestXxx`) when safe
- Use `t.Cleanup` to register teardown logic (e.g., closing resources) rather than raw `defer`
- When doing setup/teardown at package scope, use `TestMain(m *testing.M)`; but prefer per-test setup unless truly necessary
- Use `t.Setenv` to temporarily override environment variables inside a test
- Use a `testdata/` folder to store fixture files or golden outputs; Go tooling ignores `testdata/` automatically

## Assertions with testify

### Default Assertion Libraries

- **Default to using `assert` and `require` from `github.com/stretchr/testify`** for all assertions
- These packages provide clear, readable assertions with helpful error messages
- Avoid using `t.Fatalf`, `t.Errorf`, or manual error checking when `assert`/`require` would be clearer

### When to Use assert vs require

- Use **`require`** when a failure should stop the test immediately:
  - Setup failures that make the rest of the test invalid
  - Nil pointer checks before dereferencing
  - Critical preconditions that must pass
- Use **`assert`** when you want to continue checking other conditions after a failure:
  - Multiple independent assertions in the same test
  - Verifying multiple fields of a struct
  - Non-critical checks that don't block further testing

### Import Pattern

```go
import (
    "testing"
    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/require"
)
```

### Common Assertion Patterns

- Use `assert.Equal(t, expected, actual)` for equality checks
- Use `assert.NoError(t, err)` and `require.NoError(t, err)` for error checks
- Use `assert.ErrorIs(t, err, expectedErr)` for specific error type checks
- Use `assert.Contains(t, haystack, needle)` for substring/element checks
- Use `assert.Len(t, collection, expectedLength)` for length checks
- Use `assert.True(t, condition)` and `assert.False(t, condition)` for boolean checks

## What to Generate in Tests

When writing or generating tests, include:

- **Table-driven unit tests** covering normal, boundary, error, and edge cases
- **Subtests** using `t.Run(...)` with descriptive case names that explain what's being tested
- Use of `t.Parallel()` inside subtests when they don't interfere with each other
- **Assertions using `assert` and `require`** from testify instead of manual error checking
- Use of `assert.ErrorIs` / `require.ErrorIs` when verifying specific error types
- Minimal mocking/stubbing: prefer simple custom fakes instead of heavy mock libraries unless a specific interface is complex
- For outputs with potentially large content (JSON, text, etc.), compare against golden files stored under `testdata/`
- HTTP handler or client code tests using `net/http/httptest` (NewRequest, ResponseRecorder)
- Tests that avoid global shared mutable state; if used, reset or isolate state between tests

## What to Avoid in Tests

Do not generate or write:

- Using `t.Fatalf`, `t.Errorf`, or manual error checking when `assert`/`require` would be clearer
- Overly complex or deeply layered test frameworks unless project convention requires it
- Blind use of reflection, `unsafe`, or meta-programming in test code
- Tests that rely on external services (database, network, etc.) — focus on unit tests only
- Silent swallowing of errors — all unexpected error conditions should cause test failure with clear message
- Implicit or hidden setup logic — tests should declare their dependencies clearly
- Global state that is mutated across tests without resetting it
- Tests that depend on execution order
- Hard-coded absolute paths or environment-specific assumptions

## Test Helpers

### Helper Function Guidelines

- Mark helper functions with `t.Helper()` so test failures point to the correct line
- Create test fixtures for complex setup
- Use `testing.TB` interface for functions used in both tests and benchmarks
- Clean up resources using `t.Cleanup()` to ensure cleanup happens even if test fails

### Example Helper Pattern

```go
func setupTestServer(t *testing.T) *Server {
    t.Helper()

    server := NewServer()
    require.NotNil(t, server)

    t.Cleanup(func() {
        server.Close()
    })

    return server
}
```

## Example Test Templates

### Table-Driven Unit Test

```go
func TestSomething(t *testing.T) {
    cases := []struct {
        name    string
        input   InputType
        want    OutputType
        wantErr bool
    }{
        {
            name:    "normal case",
            input:   InputType{...},
            want:    OutputType{...},
            wantErr: false,
        },
        {
            name:    "edge case - empty input",
            input:   InputType{},
            want:    OutputType{...},
            wantErr: false,
        },
        {
            name:    "error case - invalid input",
            input:   InputType{...},
            wantErr: true,
        },
    }

    for _, tc := range cases {
        tc := tc // capture loop variable
        t.Run(tc.name, func(t *testing.T) {
            t.Parallel()

            got, err := MyFunc(tc.input)

            if tc.wantErr {
                require.Error(t, err)
                return
            }

            require.NoError(t, err)
            assert.Equal(t, tc.want, got)
        })
    }
}
```

### Test with Setup and Cleanup

```go
func TestWithCleanup(t *testing.T) {
    // Setup
    tmpFile, err := os.CreateTemp("", "test-*.txt")
    require.NoError(t, err)

    // Register cleanup
    t.Cleanup(func() {
        os.Remove(tmpFile.Name())
    })

    // Test logic
    result, err := ProcessFile(tmpFile.Name())
    require.NoError(t, err)
    assert.NotEmpty(t, result)
}
```

## Testing HTTP Handlers (Unit Tests)

### HTTP Handler Unit Testing

**Note:** This section covers **unit testing** of HTTP handlers using `httptest`, not integration testing. These tests run entirely in-memory without starting real servers or making network calls. They are safe to run in any environment and do not interact with external services.

HTTP handlers can be unit tested without starting a real server or making network calls. Use `net/http/httptest` to test handlers in isolation:

- `httptest.NewRequest` creates an HTTP request for testing
- `httptest.ResponseRecorder` captures the handler's response
- These are **unit tests** that run in-memory with no external dependencies
- They are safe, fast, and deterministic

### HTTP Handler Test Example

```go
func TestHTTPHandler(t *testing.T) {
    cases := []struct {
        name           string
        method         string
        path           string
        body           string
        expectedStatus int
        expectedBody   string
    }{
        {
            name:           "successful GET",
            method:         "GET",
            path:           "/api/resource",
            expectedStatus: http.StatusOK,
            expectedBody:   "success",
        },
        {
            name:           "not found",
            method:         "GET",
            path:           "/api/nonexistent",
            expectedStatus: http.StatusNotFound,
        },
    }

    for _, tc := range cases {
        tc := tc
        t.Run(tc.name, func(t *testing.T) {
            t.Parallel()

            req := httptest.NewRequest(tc.method, tc.path, strings.NewReader(tc.body))
            rec := httptest.NewRecorder()

            handler := MyHandler()
            handler.ServeHTTP(rec, req)

            assert.Equal(t, tc.expectedStatus, rec.Code)
            if tc.expectedBody != "" {
                assert.Contains(t, rec.Body.String(), tc.expectedBody)
            }
        })
    }
}
```

## Testing Policy and Mocking Guidelines

### Current Testing Policy

- **Focus exclusively on unit tests** — fast, deterministic, and isolated tests
- **Do not add integration tests** that depend on external services (databases, APIs, networks, etc.)
- If an integration scenario feels essential, coordinate with maintainers before proceeding so we can revisit the policy and document the required setup

### Mocking Guidelines

- Prefer dependency injection and interface-based design to enable testability
- Use small, focused interfaces that are easy to implement as test doubles
- Create simple hand-rolled fakes or stubs for most cases
- Only use generated mocks (like `gomock`) for complex interfaces with many methods
- Document what behavior the mock is simulating

## Golden File Testing

### When to Use Golden Files

- Use for testing complex output formats (JSON, XML, HTML, etc.)
- Store expected outputs in `testdata/` directory
- Name files descriptively: `testdata/TestName_casename.golden.json`
- Golden files should be committed to version control as test fixtures

### Golden File Pattern

**Note:** Golden files are version-controlled test fixtures representing expected output. The pattern below shows how to regenerate them during test development or when expected output legitimately changes:

```go
// Define a package-level flag for updating golden files during test development
var updateGolden = flag.Bool("update", false, "update golden test files")

func TestGenerateOutput(t *testing.T) {
    result := GenerateOutput(input)

    goldenFile := filepath.Join("testdata", "expected_output.golden.json")

    // Use -update flag during development to regenerate golden files
    // Example: go test -update
    if *updateGolden {
        err := os.WriteFile(goldenFile, result, 0644)
        require.NoError(t, err)
        t.Log("Updated golden file:", goldenFile)
    }

    expected, err := os.ReadFile(goldenFile)
    require.NoError(t, err)

    // Compare actual output against golden file
    assert.JSONEq(t, string(expected), string(result))
}
```

**Usage:**
- Normal test runs: `go test` — compares output against committed golden files
- Update golden files: `go test -update` — regenerates golden files when expected output changes
- Always review golden file changes before committing to ensure they represent intentional updates

## Common Testing Patterns

### Testing Errors

```go
// Specific error type
err := MyFunc()
assert.ErrorIs(t, err, ErrNotFound)

// Any error
err := MyFunc()
require.Error(t, err)

// No error
err := MyFunc()
require.NoError(t, err)
```

### Testing Panics

```go
assert.Panics(t, func() {
    MyFuncThatPanics()
})

assert.NotPanics(t, func() {
    MySafeFunc()
})
```

### Testing Timeouts

```go
done := make(chan bool)
go func() {
    MyLongRunningFunc()
    done <- true
}()

select {
case <-done:
    // Success
case <-time.After(5 * time.Second):
    t.Fatal("test timed out")
}
```

## Summary

Good tests are:
- **Clear**: Easy to understand what's being tested and why
- **Focused**: Test one thing at a time
- **Independent**: Don't rely on other tests or external state
- **Repeatable**: Same result every time
- **Fast**: Run quickly to encourage frequent execution
- **Maintainable**: Easy to update when requirements change

Use testify's `assert` and `require` packages as your default assertion approach, and favor simple, straightforward test code over clever abstractions.

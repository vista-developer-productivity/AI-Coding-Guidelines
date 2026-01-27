---
description: "Go testing standards. For comprehensive testing patterns, examples, and best practices, invoke the go-expert skill."
applyTo: "**/*_test.go"
---

# Go Testing Standards

## Core Principles

- Write **table-driven tests** with `t.Run` for multiple test cases
- Use **testify assertions** (`assert`/`require`) for clear, readable tests
- Keep tests **simple, deterministic, and fast** - focus on unit tests only
- Mark helper functions with **`t.Helper()`**
- Use **`t.Cleanup()`** for resource cleanup
- Test files must end in **`_test.go`** and live next to the code they test

## Essential Standards

- **Naming**: `Test_functionName_scenario` for test functions
- **Assertions**: Use `require` for critical checks, `assert` for non-blocking checks
- **Structure**: Table-driven tests with descriptive case names
- **Parallelization**: Use `t.Parallel()` in subtests when safe
- **Fixtures**: Store test data in `testdata/` directory
- **HTTP Testing**: Use `httptest` for handler unit tests (no real servers)
- **Mocking**: Prefer simple hand-rolled fakes over complex mock frameworks

---

**For comprehensive Go testing patterns, HTTP handler examples, golden file testing, and detailed best practices, invoke the `go-expert` skill.**

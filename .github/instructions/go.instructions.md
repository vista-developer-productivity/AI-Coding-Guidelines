---
description: "Go coding standards. For detailed patterns, troubleshooting, and best practices, invoke the go-expert skill."
applyTo: "**/*.go,**/go.mod,**/go.sum"
---

# Go Coding Standards

Follow idiomatic Go practices based on [Effective Go](https://go.dev/doc/effective_go) and [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments).

## Core Principles

- Write **simple, clear, and idiomatic** Go code
- Keep the **happy path left-aligned** - return early to reduce nesting
- Make the **zero value useful**
- Always use **`gofmt`** and **`goimports`** for formatting
- **Check all errors** - never ignore with `_` without documentation
- **Document all exported** types, functions, methods, and packages

## Essential Standards

- **Naming**: `mixedCaps` for variables/functions, `MixedCaps` for exports, lowercase single-word packages
- **Interfaces**: Small and focused (1-3 methods), use `-er` suffix (`Reader`, `Writer`)
- **Error Handling**: Return errors as last value, use `fmt.Errorf` with `%w` for wrapping, lowercase messages
- **Concurrency**: Close channels from sender, use `defer` for cleanup, prevent goroutine leaks
- **Testing**: Table-driven tests, use `t.Run` for subtests, name tests `Test_functionName_scenario`
- **Project Structure**: Use `cmd/` for main packages, `internal/` for private packages, Go modules for dependencies

---

**For detailed Go patterns, concurrency best practices, and troubleshooting guidance, invoke the `go-expert` skill.**

---
description: "Python coding standards. For detailed patterns, troubleshooting, and best practices, invoke the python-expert skill."
applyTo: "**/*.py"
---

# Python Coding Standards

## Core Principles

- Follow **PEP 8** style guide for all Python code
- Use **type hints** for all function parameters and return values
- Write **docstrings** for all public functions, classes, and modules (PEP 257)
- Prioritize **readability** and **simplicity** over cleverness
- Keep functions **small and focused** (single responsibility)
- Handle **errors explicitly** - never silently ignore exceptions
- Write **unit tests** for all critical code paths

## Essential Standards

- **Formatting**: Use 4 spaces for indentation, max line length 79 characters
- **Naming**: `snake_case` for functions/variables, `PascalCase` for classes, `UPPER_CASE` for constants
- **Type Annotations**: Use `typing` module (`List[str]`, `Dict[str, int]`, `Optional[T]`)
- **Documentation**: Start docstrings with one-line summary, include Parameters and Returns sections
- **Error Handling**: Use specific exception types, provide meaningful error messages
- **Testing**: Write descriptive test names, test edge cases, use pytest for testing

---

**For detailed Python development patterns, testing strategies, and troubleshooting guidance, invoke the `python-expert` skill.**

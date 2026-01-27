---
description: "TypeScript and JavaScript coding standards"
applyTo: "**/*.ts, **/*.js, **/*.jsx, **/*.tsx"
---

# TypeScript and JavaScript Coding Standards [v1.0]

Minimal standards for TypeScript/JavaScript. For comprehensive React, TypeScript, and frontend guidance, invoke the `frontend-expert` skill.

## Configuration Precedence

1. **Project config files** (`.prettierrc`, `.eslintrc`, `stylelint.config.js`) - Always take precedence
2. **This document** - Apply when no project config exists
3. **IDE defaults** - Fallback only

## Naming Conventions

- **Variables**: camelCase (`userName`, `totalAmount`)
- **Constants**: SCREAMING_SNAKE_CASE (`MAX_RETRY_ATTEMPTS`)
- **Functions**: camelCase with verb prefixes (`getUserData`, `validateInput`)
- **Classes**: PascalCase (`UserValidator`, `PaymentProcessor`)
- **Files**: kebab-case (`user-profile.component.ts`)
- **Interfaces/Types**: PascalCase (`UserData`, `ResultType`)
- **Enums**: PascalCase (`StatusCode`, `UserRole`)

## Code Style

- Use Prettier if configured, ESLint for linting
- Treat warnings as errors
- **Functions**: Under 20 lines when possible, max 3-4 parameters
- **Single Responsibility**: Each function does one thing
- **Return Types**: Always specify in TypeScript
- **Async/Await**: Preferred over Promises
- **Immutability**: Avoid mutation, use spread operators
- **Early Returns**: Use guard clauses to minimize nesting

## TypeScript Essentials

- **Strict Mode**: Always enabled
- **Avoid `any`**: Use `unknown` or proper types
- **Null Safety**: Handle null/undefined explicitly
- **Interfaces**: Use for reusable, complex structures
- **Type Aliases**: Use for simple, single-use types
- **Type Guards**: Implement for runtime type checking

```typescript
// Good: Interface for complex reusable structure
interface UserData {
  readonly id: string;
  name: string;
  email: string;
}

// Good: Inline type for simple single-use
function Button(props: { text: string; onClick: () => void }) {}

// Good: Type guard
const isUserData = (value: unknown): value is UserData => {
  return typeof value === "object" && value !== null && "id" in value;
};
```

## Import Organization

1. Node modules (external dependencies)
2. Internal modules (absolute paths with `@/`)
3. Relative imports (local files)

## Error Handling

- Use Result/Either pattern for errors
- Structured logging with correlation IDs
- Never expose sensitive data in errors
- Use appropriate log levels

## Performance

- Implement code splitting and lazy loading
- Use tree-shaking friendly imports (specific, not entire libraries)
- Monitor bundle sizes
- Set bundle size budgets

## Security & Accessibility

- Use `eslint-plugin-jsx-a11y` for accessibility
- Validate and sanitize all inputs
- Use security-focused ESLint rules
- Maintain WCAG AA contrast ratios (4.5:1)

## Comprehensive Guidance

For detailed React patterns, component architecture, testing strategies, and frontend optimization, invoke the **`frontend-expert` skill**.

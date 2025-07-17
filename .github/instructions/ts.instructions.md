---
description: 'Documentation and content creation standards'
applyTo: '**/*.ts, **/*.js", **/*.jsx, **/*.tsx'
---

# TypeScript and JavaScript Coding Standards

## Formatting and Style
- **Prettier**: Use Prettier for consistent code formatting
  - Configure via `.prettierrc` with project-specific rules. 
  - Run formatting on save and in pre-commit hooks
  - Ensure consistent indentation, line breaks, and spacing

## Formatting and Style
- **Prettier**: If it exists, follow the project’s `.prettierrc` for consistent code formatting.
  - Run formatting on save.
  - Ensure consistent indentation, line breaks, and spacing.
- **ESLint**: If it exists, follow `.eslintrc` for linting, treating warnings as errors.
  - Key rules: `@typescript-eslint/no-unused-vars`, `no-console`, `eqeqeq`, `curly`.
- **Stylelint**: Use Stylelint for CSS/SCSS, enforcing accessibility and consistency.

### **Code Structure Rules**

#### **Naming Conventions**
- Use descriptive names (e.g., `calculateTotalPrice` instead of `calc`)
- **Variables**: camelCase (`userName`, `totalAmount`)
- **Constants**: SCREAMING_SNAKE_CASE (`MAX_RETRY_ATTEMPTS`)
- **Functions**: camelCase with verb prefixes (`getUserData`, `validateInput`)
- **Classes**: PascalCase (`UserValidator`, `PaymentProcessor`)
- **Files**: kebab-case for components (`user-profile.component.ts`)
- **Interfaces**: PascalCase with optional 'I' prefix (`UserData` or `IUserData`)
- **Types**: PascalCase (`ResultType`, `ValidationError`)
- **Enums**: PascalCase (`StatusCode`, `UserRole`)

#### **Function and Class Design**
- **Single Responsibility**: Each function should do one thing well
- **Function Length**: Keep functions under 20 lines when possible
- **Parameter Limits**: Maximum 3-4 parameters; use objects for more
- **Return Types**: Always specify return types explicitly in TypeScript
- **Pure Functions**: Prefer pure functions without side effects
- **Immutability**: Avoid mutation; favor immutable data structures and operations
- **Async/Await**: Use async/await over Promises for better readability
- **Avoid Nested Ternary**: Use if/else statements for complex conditional logic
- **Simple Predicates**: Avoid complex logic within predicates; extract to named methods
- **Flat Structure**: Minimize indentation levels; prefer early returns and guard clauses

```typescript
// Good: Simple, readable conditional logic
const getStatusMessage = (status: Status): string => {
  if (status === 'active') return 'User is active'
  if (status === 'inactive') return 'User is inactive'
  return 'Unknown status'
}

// Avoid: Nested ternary expressions
const getStatusMessage = (status: Status): string => 
  status === 'active' ? 'User is active' 
    : status === 'inactive' ? 'User is inactive' 
    : 'Unknown status'

// Good: Immutable operations
const addUser = (users: User[], newUser: User): User[] => {
  return [...users, newUser]
}

// Avoid: Mutation
const addUser = (users: User[], newUser: User): User[] => {
  users.push(newUser) // Mutates original array
  return users
}

// Good: Early return pattern
const processUser = (user: User): ProcessedUser => {
  if (!user.email) {
    throw new ValidationError('Email is required')
  }
  
  if (!user.isActive) {
    throw new ValidationError('User must be active')
  }
  
  return transformUser(user)
}
```

### **Error Handling Standards**

#### **TypeScript/JavaScript Error Handling**
```typescript
// Use Result/Either pattern for error handling
type Result<T, E = Error> = { success: true; data: T } | { success: false; error: E }

// Always handle async operations properly
const processData = async (input: string): Promise<Result<ProcessedData, ValidationError>> => {
  try {
    const result = await riskyOperation(input)
    return { success: true, data: result }
  } catch (error) {
    // Log error with context
    logger.error('Operation failed', { 
      error: error.message, 
      input: sanitizeForLogging(input),
      correlationId: getCorrelationId() 
    })
    return { success: false, error: new ValidationError('Processing failed', error) }
  }
}
```

#### **Error Logging Requirements**
- Use structured logging with consistent format
- Include correlation IDs for request tracing
- Never expose sensitive data in error messages
- Provide actionable error messages to users
- Use appropriate log levels (ERROR, WARN, INFO, DEBUG)

### **Import and Dependency Management**

#### **Import Organization**
```typescript
// 1. Node modules (external dependencies)
import { useState, useEffect } from 'react'
import axios from 'axios'
import { z } from 'zod'

// 2. Internal modules (absolute paths)
import { UserService } from '@/services/user-service'
import { validateInput } from '@/utils/validation'
import type { ApiResponse } from '@/types/api'

// 3. Relative imports (local to current module)
import { Button } from '../components/button'
import { useLocalState } from './hooks/use-local-state'
import './component.styles.css'
```

#### **Dependency Rules**
- Declare dependencies accurately in `package.json`
- Specify peer dependencies as ranges (e.g., `>=17 <19`)
- Avoid `--legacy-peer-deps` flag
- Regular dependency audits and updates using `npm audit`
- Use exact versions for critical dependencies
- Separate dev dependencies from production dependencies
- Document dependency choices in architecture decisions

### **Type Safety Requirements**

#### **TypeScript Best Practices**
- **Strict Mode**: Always use TypeScript strict mode
- **Type Definitions**: Create explicit interfaces for all data structures
- **Avoid `any`**: Use `unknown` or proper types instead of `any`
- **Null Safety**: Handle null/undefined cases explicitly
- **Generic Constraints**: Use proper generic constraints
- **Type Guards**: Implement type guards for runtime type checking
- **Branded Types**: Use branded types for domain-specific values

```typescript
// Good: Explicit interface with proper typing
interface UserData {
  readonly id: string
  name: string
  email: string
  createdAt: Date
  preferences?: UserPreferences
}

// Good: Proper error handling with types
const processUser = (userData: UserData): Result<ProcessedUser, ValidationError> => {
  if (!isValidEmail(userData.email)) {
    return { success: false, error: new ValidationError('Invalid email format') }
  }
  
  const processedUser = transformUserData(userData)
  return { success: true, data: processedUser }
}

// Good: Type guard implementation
const isUserData = (value: unknown): value is UserData => {
  return typeof value === 'object' && 
         value !== null && 
         'id' in value && 
         'name' in value && 
         'email' in value
}
```

### **Performance and Bundle Optimization**

#### **Code Splitting and Lazy Loading**
- Implement route-based code splitting
- Lazy load heavy components and libraries
- Use dynamic imports for conditional features
- Monitor bundle size with webpack-bundle-analyzer or similar tools
- Set bundle size budgets and fail builds if exceeded

#### **Import Optimization**
- Use tree-shaking friendly imports
- Avoid importing entire libraries when only using specific functions
- Use barrel exports judiciously to avoid circular dependencies
- Prefer named imports over default imports for better tree-shaking

```typescript
// Good: Specific imports for better tree-shaking
import { debounce } from 'lodash-es/debounce'
import { format, parseISO } from 'date-fns'

// Good: Conditional imports for performance
const HeavyComponent = lazy(() => import('./HeavyComponent'))

// Avoid: Full library imports
import * as _ from 'lodash'
import * as dateFns from 'date-fns'
```

### **Accessibility and Security Linting**

#### **Accessibility Rules**
- Use `eslint-plugin-jsx-a11y` for React accessibility linting
- Ensure proper ARIA labels and roles
- Maintain logical tab order with `tabindex`
- Provide alternative text for images and media
- Use semantic HTML elements
- Test with screen readers and keyboard navigation
- Maintain color contrast ratios (WCAG AA: 4.5:1, AAA: 7:1)

#### **Security-Focused ESLint Rules**
- `security/detect-object-injection`: Prevent object injection vulnerabilities
- `security/detect-non-literal-regexp`: Avoid dynamic regex construction
- `security/detect-unsafe-regex`: Detect potentially unsafe regular expressions
- `security/detect-buffer-noassert`: Prevent buffer overflow vulnerabilities
- `security/detect-eval-with-expression`: Prevent eval usage
- `security/detect-no-csrf-before-method-override`: CSRF protection

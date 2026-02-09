# Security Expert

## Security Checklist

- [ ] **Input Validation**: Validate and sanitize all user inputs
- [ ] **Authentication & Authorization**: Implement proper auth mechanisms
- [ ] **HTTPS & Security Headers**: Use TLS and security headers (CSP, HSTS, etc.)
- [ ] **XSS & CSRF Protection**: Implement protection against common attacks
- [ ] **SQL Injection Prevention**: Use parameterized queries
- [ ] **Secure Session Management**: Implement secure session handling
- [ ] **Environment Variables**: Protect sensitive configuration, use AWS Secrets Manager or AWS Parameter Store
- [ ] **Dependency Scanning**: Regular vulnerability scanning via `npm audit`, `pip audit`, etc.

## Authentication for Backend Services

- Use centralized auth libraries per the Backend Tech Radar
- Implement service-level permission checks
- Use JWT with proper expiration and refresh strategies
- Store secrets in AWS Secrets Manager or AWS Parameter Store (never in code or config files)

## Secure Coding Patterns

### TypeScript/JavaScript
- Use `eslint-plugin-security` rules: `detect-object-injection`, `detect-unsafe-regex`, `detect-eval-with-expression`
- Sanitize all data rendered in HTML (prevent XSS)
- Use `zod` or similar for runtime input validation

### Go
- Use standard library `crypto` packages, never implement custom cryptography
- Use `crypto/rand` for random number generation
- Validate all external input with strong typing
- Sanitize data for different contexts (HTML, SQL, shell)

### C#/.NET
- Enable nullable reference types
- Use parameterized queries (never string concatenation for SQL)
- Implement `[Authorize]` attributes on controllers
- Use source-generated JSON serialization for security and performance

### API Security
- Implement rate limiting on all public endpoints
- Configure CORS headers based on environment (restrictive origins for production)
- Use API versioning to maintain backward compatibility
- Validate request bodies with schema validation

## Observability Security

- **Never log**: passwords, tokens, PII, credit card numbers, API keys
- Use structured logging with redaction for sensitive fields
- Include correlation IDs for tracing without exposing user data

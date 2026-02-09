# API Design Standards

## RESTful Principles

- Follow RESTful principles and appropriate HTTP status codes
- Use consistent naming conventions: **kebab-case for URLs** (`/api/user-profiles`)
- Implement proper versioning strategy (`/api/v1/...`)
- Document APIs with **OpenAPI/Swagger**
- Implement rate limiting and authentication on all endpoints

## HTTP Methods & Status Codes

| Method | Use For | Success Code |
|--------|---------|-------------|
| GET | Read resources | 200 OK |
| POST | Create resources | 201 Created |
| PUT | Full update | 200 OK |
| PATCH | Partial update | 200 OK |
| DELETE | Remove resources | 204 No Content |

Common error codes: 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Unprocessable Entity, 429 Too Many Requests, 500 Internal Server Error.

## CORS Configuration

- Configure CORS headers based on environment
- Use restrictive origins for production (explicit allow-list)
- Use permissive origins for development only
- Always specify allowed methods and headers explicitly

## Request/Response Patterns

- Use request validation (zod for TypeScript, pydantic for Python, struct tags for Go)
- Return consistent error response format across all endpoints
- Use pagination for list endpoints (`?page=1&limit=20` or cursor-based)
- Include `Content-Type` headers on all responses
- Support `Accept` header content negotiation where appropriate

## Framework Selection (per Backend Tech Radar)

| Framework | Status | Language |
|-----------|--------|----------|
| **Fastify** | ✅ Adopt (preferred) | TypeScript/Node.js |
| **ExpressJS** | ⚠️ Trial | TypeScript/Node.js |
| **FastAPI** | ✅ Adopt | Python |
| **Gin** | ✅ Adopt | Go |
| NestJS | ❌ Hold | TypeScript |
| Java SpringBoot | ❌ Hold | Java |

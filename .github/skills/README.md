# Vista Copilot Skills

This directory contains **on-demand skills** for GitHub Copilot. Unlike the `.github/AGENTS.md` file (which is loaded into context for every prompt), skills are loaded **only when invoked**, reducing context window usage and improving suggestion quality.

## How Skills Work

| Layer | File | Context Impact |
|-------|------|---------------|
| **AGENTS.md** | `.github/AGENTS.md` | ⚠️ Sent with every request (high context cost) |
| **Skills** | `.github/skills/*.md` | ✅ Loaded on-demand (context efficient) |
| **Instructions** | `.github/instructions/*.md` | Auto-loaded by file type via `applyTo` |

## Available Skills

| Skill | File | Use For |
|-------|------|---------|
| Vista Engineering Principles | `vista-engineering-principles.md` | Design discussions, architecture reviews |
| Testing Standards | `testing-standards.md` | Writing/reviewing tests, coverage strategy |
| Security Expert | `security-expert.md` | Auth, vulnerabilities, secure coding |
| Observability | `observability.md` | Logging, monitoring, tracing |
| Documentation Standards | `documentation-standards.md` | README, ADR, RFC, CHANGELOG |
| CI/CD & Deployment | `cicd-deployment.md` | Pipelines, deployment, version control |
| Performance Optimization | `performance-optimization.md` | Caching, bundle size, DB optimization |
| API Design | `api-design.md` | REST design, CORS, rate limiting |
| Currency Handling | `currency-handling.md` | Monetary values, ISO 4217 |

## How to Invoke

In Copilot Chat, reference a skill by name. Example:

```
/security-expert Review this authentication flow for vulnerabilities
```

## Contributing

To add a new skill:
1. Create a new `.md` file in this directory
2. Keep it focused on one domain (single responsibility)
3. Include Tech Radar alignment where applicable
4. Submit an MR for review by the relevant domain expert

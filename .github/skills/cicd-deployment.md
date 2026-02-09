# CI/CD & Deployment Standards

## Version Control

- Use **SemVer** (MAJOR.MINOR.PATCH, e.g., `1.2.3`), avoiding unnecessary major version changes
- Write clear, descriptive commit messages following **conventional commits** format
- Commit frequently with atomic changes
- Use meaningful branch names: `feature/description`, `bugfix/description`
- Follow branching strategies (Git Flow, GitHub Flow) consistently
- Conduct code reviews checking for logic, security, performance, and maintainability before merging

## CI/CD Pipeline

- Automate builds, tests, and deployments via CI/CD (**GitLab CI** or **GitHub Actions**)
- Target **15-minute deployment cycles** for rapid feedback
- Implement quality gates: tests, linting, security scans
- Use environment-specific configurations
- Implement rollback strategies for failed deployments

## TypeScript/JavaScript Specifics

- Use recommended `.npmrc` template
- Avoid `--legacy-peer-deps` flag
- Run `npm audit` as part of CI pipeline
- Set bundle size budgets and fail builds if exceeded

## Deployment Targets (per Backend Tech Radar)

| Target | Status | Notes |
|--------|--------|-------|
| **AWS Lambda** | ✅ Adopt | Preferred for event-driven, stateless workloads |
| **AWS ECS/Fargate** | ✅ Adopt | Preferred for long-running services |
| **Docker** | ✅ Adopt | Standard containerization (see Docker skill) |

## Infrastructure as Code

- **AWS CDK** (preferred), Terraform, or Pulumi for infrastructure
- Version control all infrastructure definitions
- Use separate stacks/modules for different environments
- Implement drift detection

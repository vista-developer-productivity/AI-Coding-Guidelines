---
applyTo: "**/Dockerfile, **/docker-compose.yml, **/*.dockerfile"
description: "Docker and container standards. For detailed examples, workflows, and troubleshooting, invoke the iac-expert skill."
---

# Containerization & Docker Standards

These are mandatory standards for all Dockerfiles and container configurations. For deeper guidance, examples, and troubleshooting workflows, ask Copilot to use the **iac-expert** skill.

## Core Principles

- **Immutability**: Create new images for changes; never modify running containers in production
- **Portability**: Design environment-agnostic containers; externalize all configuration
- **Isolation**: Run single process per container; use container networking, not host networking
- **Efficiency**: Optimize for small images; smaller = faster builds, fewer vulnerabilities

## Dockerfile Standards

- **Multi-stage builds**: Always use for compiled languages and when build tools are heavy
- **Minimal base images**: Use `alpine`, `slim`, or `distroless` variants; avoid full distributions
- **Specific version tags**: Never use `latest` in production; pin versions (e.g., `node:18-alpine`)
- **Layer optimization**: Order from least to most frequently changing; combine RUN commands
- **Clean up in same layer**: Remove temp files in the same RUN command that creates them
- **Use .dockerignore**: Exclude `.git`, `node_modules`, build artifacts, `.env` files
- **Selective COPY**: Copy dependency files before source code for better caching
- **Non-root USER**: Always define a non-root user; create dedicated app user
- **EXPOSE for documentation**: Document expected ports even though it doesn't publish them
- **Exec form for CMD/ENTRYPOINT**: Use `["executable", "arg"]` not shell form

## Container Security Standards

- **No root**: Run as non-root user with minimum necessary permissions
- **No secrets in layers**: Never COPY/ADD secrets; use runtime secrets management
- **Minimal capabilities**: Drop unnecessary Linux capabilities (`CAP_DROP`)
- **HEALTHCHECK required**: Define health checks for orchestration systems
- **Scan images**: Integrate Hadolint (Dockerfile) and Trivy/Snyk (vulnerabilities) in CI
- **Sign production images**: Use Cosign or Docker Content Trust for image verification

## Runtime Standards

- **Resource limits**: Always set CPU and memory limits in compose/Kubernetes
- **Structured logging**: Log to STDOUT/STDERR in JSON format
- **Named volumes**: Use for persistent data; never store state in container layer
- **Custom networks**: Create isolated networks; use internal networks for backend services

## Dockerfile Review Checklist

- [ ] Multi-stage build used (if applicable)?
- [ ] Minimal, versioned base image (e.g., `alpine`, `slim`)?
- [ ] Layers optimized (combined RUN, cleanup in same layer)?
- [ ] `.dockerignore` present and comprehensive?
- [ ] `COPY` instructions specific and minimal?
- [ ] Non-root `USER` defined?
- [ ] `EXPOSE` instruction for documentation?
- [ ] `CMD`/`ENTRYPOINT` in exec form?
- [ ] No hardcoded secrets or sensitive data?
- [ ] `HEALTHCHECK` instruction defined?
- [ ] CI includes Hadolint and image vulnerability scanning?

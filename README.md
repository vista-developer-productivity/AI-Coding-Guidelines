# AI Coding Guide

This repository provides guidance for working effectively with AI-assisted development tools across our organization. As AI coding assistants become integral to our development workflow, it's essential to understand how to guide these tools to produce code that aligns with our standards and engineering best practices.

## Getting Started

**[CODING_GUIDELINES.md](CODING_GUIDELINES.md)** is your primary resource, containing practical techniques for instructing your IDE agent to behave correctly. LLMs are often very optimistic, and as an Engineer it is your responsibility to guide and coach AI agents to align with our internal guidelines and general software engineering best practices.

## Skills: On-Demand Domain Expertise

This repository uses a **three-tier knowledge system** to provide AI assistants with the right level of guidance:

| Tier | Location | Loading | Purpose |
|------|----------|---------|---------|
| **Instructions** | `.github/instructions/*.instructions.md` | Always loaded | Lightweight coding standards (~30 lines) |
| **Language Skills** | `.copilot/skills/*/SKILL.md` | On-demand | Language-specific features and idioms |
| **Domain Skills** | `.copilot/skills/*/SKILL.md` | On-demand | Comprehensive architectural patterns |

**Available Skills:**
- **[backend-expert](.copilot/skills/backend-expert/SKILL.md)** - REST APIs, microservices, async patterns
- **[data-expert](.copilot/skills/data-expert/SKILL.md)** - dbt, SQL, data modeling, analytics
- **[dotnet-expert](.copilot/skills/dotnet-expert/SKILL.md)** - C#, ASP.NET Core, Entity Framework
- **[frontend-expert](.copilot/skills/frontend-expert/SKILL.md)** - React, TypeScript, Next.js, CSS
- **[go-expert](.copilot/skills/go-expert/SKILL.md)** - Go idioms, concurrency, error handling
- **[iac-expert](.copilot/skills/iac-expert/SKILL.md)** - Terraform, Docker, Kubernetes, DevOps
- **[python-expert](.copilot/skills/python-expert/SKILL.md)** - Python idioms, type hints, stdlib
- **[testing-expert](.copilot/skills/testing-expert/SKILL.md)** - Testing patterns, pytest, mocking

Skills are automatically invoked when relevant, or you can explicitly request them: *"Using the backend-expert skill, help me design a REST API."*

See **[.copilot/skills/README.md](.copilot/skills/README.md)** for complete documentation on available skills and usage.

## Understanding AI Limitations

For engineers who want to understand *why* certain coaching techniques are necessary, we've included technical guides:

- **[AI_RECENCY_BIAS_GUIDE.md](AI_RECENCY_BIAS_GUIDE.md)** - Explains why AI assistants sometimes "forget" earlier instructions or drift from established patterns. Learn to recognize recency bias and when to reset conversations.

- **[NEGATION_AND_INSTRUCTION_CLARITY_GUIDE.md](NEGATION_AND_INSTRUCTION_CLARITY_GUIDE.md)** - Covers how to write AI-friendly instructions that are reliably understood. Explains how tokenization affects negation processing and provides patterns for clear guidance.

**Start with CODING_GUIDELINES.md for practical techniques, then read the technical guides when you need to understand the underlying science.**

## Customizing AI Behavior

We encourage all teams to share their templates for greater knowledge sharing. This repository supports multiple levels of customization:

### Instructions (Always-On Standards)
Lightweight files in `.github/instructions/` that are always loaded into context. Use for essential coding standards.

### Skills (On-Demand Expertise)
Comprehensive knowledge packs in `.copilot/skills/` that are invoked when needed. Use for deep domain expertise.

### VS Code Configuration
- **Instructions**: [Adding Repository Custom Instructions](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot?tool=vscode)
- **Customization**: [VS Code Copilot Customization](https://code.visualstudio.com/docs/copilot/copilot-customization)
- **Skills**: Place in `.copilot/skills/` directory (recommended) or `.github/skills/` for automatic detection
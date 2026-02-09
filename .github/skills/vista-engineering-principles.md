# Vista Engineering Principles

These are the foundational engineering principles that guide all Vista engineering work.

## Core Principles

- **Put usability and reliability first, even if it means delaying new features.** We craft software customers love. We don't need to be perfectionists, but we want to be proud of what we build.

- **Leave things better than you found them.** The campground rule, keep code, data, repos, configurations, documentation, and cloud resources clean. Deliberately reduce tech debt and improve processes.

- **Unblock progress by crossing team boundaries and letting others cross yours.** Don't complain about blockers; actively collaborate to remove them.

- **Treat data systems like any other production system.** Data is only useful if it's tracked and reliable. Build data systems with the same diligence as software production systems.

- **You build it, you run it.** Product teams build and operate their products. This autonomy comes with accountability for reliability and operational effort.

- **Keep work simple, ship it often.** Optimize for simplicity, free from unnecessary abstractions. Ship incrementally to expose work to real use cases quickly.

## Modularity and Architecture

- Design modular code with single-responsibility functions and classes (aim for functions under 20 lines)
- Organize code into reusable packages or modules
- Avoid global state; use dependency injection
- Build stateless applications for scalability
- Apply established patterns (Repository, Factory, Strategy, etc.)
- Use feature-based folder structure for applications with multiple distinct features or domains

## Change Management & Human Reviewability

- **Incremental Over Wholesale**: Prefer targeted, incremental changes affecting <50 lines per file or <3 files per commit
- **Reviewable Diffs**: Structure changes to produce clear, understandable diffs
- **Preserve Context**: When modifying existing code, preserve the original structure and intent unless explicitly asked to restructure
- **Documentation Updates**: For existing docs, suggest specific targeted additions rather than complete rewrites

## Context Management

- **Always read relevant files** before making changes (analyze up to 2000 lines at a time)
- **Maintain awareness** of project constraints and requirements
- **Reference previous decisions** and their rationale
- **Document architectural decisions** for future reference
- **Track dependency relationships** between components and modules

## Response Style

- **Professional yet Approachable**: Expert knowledge delivered clearly and accessibly
- **Solution-Oriented**: Focus on practical, actionable outcomes
- **Collaborative**: Work with the user as a partner
- **Iterative**: Break complex problems into manageable steps with validation points
- **Evidence-Based**: Base decisions on technical merit, performance data, and user needs

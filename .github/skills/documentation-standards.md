# Documentation Standards

## README.md Requirements

Every project must include a comprehensive `README.md` with:

- **Project overview and purpose**, What does this service/app do?
- **Setup and installation instructions**, How to get running locally
- **Usage examples and API documentation**, Key endpoints or interfaces
- **Contributing guidelines**, How to submit changes
- **Troubleshooting section**, Common issues and solutions

If `README.md` already exists, identify missing sections and suggest specific additions (don't rewrite the whole file).

## Technical Decision Documentation

These recommended document types help product teams communicate technical ideas and decisions with each other and with their stakeholders. They strengthen collaboration across the organisation by fostering consistency, creating a shared vocabulary, and promoting familiarity with the purpose of each document type.

> **Tools, not process.** These documents are ready-made tools and best practices, not rigid process. The document a team chooses will depend on their own circumstances and needs. These guidelines are purposefully open to interpretation. If a different format or process works better for your team, use it. Experimentation is encouraged. The ultimate objective is for teams to **thoughtfully document their decisions**.

### Document Types at a Glance

| Document | What is it? | When to use it? | Scope | Where? |
|----------|-------------|-----------------|-------|--------|
| **RFC** (Request for Comments) | A technical document seeking feedback from peers on a proposed approach to solving a problem | When seeking to solve a problem and need collaborative input | Product team · Domain · Org | **Confluence**, in your space, tagged `rfc` |
| **ADR** (Architecture Decision Record) | A lightweight document to capture a permanent record of a specific software design choice | When your team makes a technical decision that new developers to a codebase should be aware of | Codebase | **GitLab**, within the git repository |
| **Play / Playbook** | A collection of strategic options designed to communicate context over a longer time period so a system can evolve gradually | When you want to present long-term technical options to stakeholders or incrementally evolve a system | Domain | **Confluence**, in your space, tagged `play` |
| **Standard** | An established norm or expectation across all teams on how to solve a specific problem consistently | To avoid variations of implementation and subjective standards | Org | **Standards repository** |

---

### Architecture Decision Records (ADRs)

Record **codebase-scoped** architectural decisions that are hard to reverse. ADRs live in the git repository so they travel with the code.

- Store in `docs/decisions/YYYY-MM-DD-Decision-Title.md`
- Include: Context, Decision, Consequences, Status
- Record all significant technical decisions for future reference

#### When to write an ADR

- Choosing or replacing a framework, library, or data store
- Changing API contracts, authentication flows, or deployment topology
- Adopting a new pattern (e.g., CQRS, event sourcing) within a service
- Any decision a future engineer would ask "why did we do this?"

#### ADR template

Format: `docs/decisions/YYYY-MM-DD-Decision-Title.md`

```markdown
# ADR-NNN: <Title>
**Status**: Proposed | Accepted | Superseded by ADR-XXX  **Date**: YYYY-MM-DD
## Context, What problem prompted this decision?
## Decision, What did we decide, and why?
## Consequences, What trade-offs or follow-up work does this create?
```

---

### Requests for Comments (RFCs)

Use an RFC when a decision **crosses team boundaries** or affects the wider organisation. RFCs are **proposals**, they seek collaborative feedback before commitment.

- Store in Confluence, in your team's space, tagged with `rfc`
- Once accepted, capture the outcome as an ADR in each affected git repository

#### When to write an RFC (instead of an ADR)

| Signal | Example |
|--------|---------|
| Multiple teams affected | Shared API contract change, new org-wide library |
| Org-wide standard change | Adopting OpenTelemetry, switching from Cypress to Playwright |
| Significant cost/risk | New cloud service, major migration |
| Reversibility is low | Data model changes across services |

#### RFC template

Store in Confluence, tagged `rfc`. Structure:

```markdown
# RFC: <Title>
## Summary, One-paragraph overview of the proposal
## Status, Draft | In Review | Accepted | Rejected
## Play Owners, Author(s), Reviewer(s), Approver(s)
## Problem Diagnosis, What is the problem? Evidence/metrics?
## Guiding Principles, Constraints that shaped the solution
## Coherent Actions, Specific, coordinated steps to solve the problem
## Architecture Diagrams, Current State / Future State
```

> **Plays/Playbooks** and **Standards** are described in the table above. Plays go in Confluence (tagged `play`), Standards go in the Standards repository.

## Changelog

- Maintain a `CHANGELOG.md` using **SemVer** (MAJOR.MINOR.PATCH)
- Follow [Keep a Changelog](https://keepachangelog.com/) format
- Categories: Added, Changed, Deprecated, Removed, Fixed, Security

## Operational Documentation

- Provide an `ops/README.md` for operational concerns
- Document runbooks for common operational tasks
- Include escalation paths and on-call procedures

## Code Documentation

- Write clear, self-documenting code first
- Add comments only when code cannot explain: intent, complex algorithms, important warnings
- Use language-appropriate docstring/comment formats (JSDoc, Go doc comments, Python docstrings, XML docs for C#)
- Avoid obvious documentation that restates what the code already says

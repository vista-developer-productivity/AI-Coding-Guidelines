# Migration: copilot.instructions.md → AGENTS.md + Skills

This document describes the migration from a single monolithic `copilot.instructions.md` to the 3-layer context model (`AGENTS.md` + Skills + Instructions).

## Why

The current `copilot.instructions.md` (241 lines, ~4,800 tokens) is loaded into context for **every prompt**. Most content is only relevant for specific tasks. This wastes context window budget and reduces the quality of Copilot suggestions.

## What Changes

### Before
```
.github/instructions/copilot.instructions.md  (241 lines, always loaded)
.github/instructions/ts.instructions.md       (auto-loaded for *.ts)
.github/instructions/go.instructions.md        (auto-loaded for *.go)
... other language-specific files
```

### After
```
.github/AGENTS.md                                (86 lines, always loaded, 64% smaller)
.github/skills/vista-engineering-principles.md   (on-demand)
.github/skills/testing-standards.md              (on-demand)
.github/skills/security-expert.md                (on-demand)
.github/skills/observability.md                  (on-demand)
.github/skills/documentation-standards.md        (on-demand)
.github/skills/cicd-deployment.md                (on-demand)
.github/skills/performance-optimization.md       (on-demand)
.github/skills/api-design.md                     (on-demand)
.github/skills/currency-handling.md              (on-demand)
.github/instructions/ts.instructions.md          (unchanged)
.github/instructions/go.instructions.md          (unchanged)
... other language-specific files (unchanged)
```

## Context Savings

```
BEFORE: ~4,800 tokens per prompt (copilot.instructions.md always loaded)
AFTER:  ~1,600 tokens per prompt (.github/AGENTS.md always loaded)
SAVING: ~3,200 tokens per prompt (67% reduction)
```

## Tech Radar Gaps Fixed

The migration also aligns the instructions with the latest Tech Radars (Backend Nov 2025, GenAI Jan 2026, Frontend E2E Jan 2026):

- Java flagged as Hold (was unlabeled)
- Express flagged as Trial, Fastify as preferred (was unlabeled)
- NestJS flagged as Hold (was missing)
- Playwright added for E2E testing (was missing)
- Cypress/Selenium flagged as Hold (was missing)
- OpenTelemetry added as Trial (was missing)
- DataDog flagged as Hold (was missing)
- Sentry, Wormly, Catchpoint, Tailscale flagged as Remove (was missing)
- Bugsnag confirmed as Adopt (was mislabeled)
- New Relic One added as Adopt (was missing)
- Valkey added alongside Redis (was missing)
- MongoDB flagged as Hold (was missing)
- MCP and AWS Bedrock added for GenAI (was missing)

## MR Rollout Plan

| Phase | MR | Branch | Content |
|-------|----|--------|---------|
| 1, Foundation | #1 | `feat/agents-md-foundation` | `.github/AGENTS.md`, `.github/skills/README.md`, this migration plan |
| 2, Skills | #2–#10 | `feat/skill-*` (one per skill) | Individual skill files, reviewed by domain experts |
| 3, Cutover | #11 | `feat/agents-md-cutover` | Deprecate old `copilot.instructions.md`, update docs |

Phase 2 MRs are independent of each other and can be reviewed/merged in any order.

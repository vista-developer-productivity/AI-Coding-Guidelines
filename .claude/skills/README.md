# Claude Skills for AI-Assisted Coding

This directory contains Claude Skills - specialized knowledge packs that provide on-demand expertise for specific development domains. Unlike instruction files that are always loaded into context, skills are invoked when needed, making them more context-efficient.

## Available Skills

| Skill                                         | Use For                                                                       |
| --------------------------------------------- | ----------------------------------------------------------------------------- |
| [Frontend Expert](./frontend-expert/SKILL.md) | React, TypeScript, Next.js, UI components, CSS, performance, accessibility    |
| [Go Expert](./go-expert/SKILL.md)             | Go development, concurrency, testing, HTTP handlers, idiomatic Go patterns    |
| [IaC Expert](./iac-expert/SKILL.md)           | Terraform, AWS CDK, CloudFormation, Pulumi, Docker, Kubernetes, DevOps        |
| [Python Expert](./python-expert/SKILL.md)     | Python development, type hints, pytest, async, OOP, decorators, best practices |

## Directory Structure

Each skill is stored in a folder with a `SKILL.md` file containing frontmatter metadata:

```
.claude/
└── skills/
    ├── README.md                 # This file
    ├── frontend-expert/
    │   └── SKILL.md             # React & frontend expertise
    ├── go-expert/
    │   └── SKILL.md             # Go development expertise
    ├── iac-expert/
    │   └── SKILL.md             # IaC & containerization expertise
    └── python-expert/
        └── SKILL.md             # Python development expertise
```

## How to Use Skills

### Automatic Skill Invocation

Skills are automatically loaded when relevant to your task based on their descriptions. Simply work with code or ask questions, and the appropriate skill will be invoked:

- Working with React components → `frontend-expert` loads automatically
- Writing Go tests → `go-expert` loads automatically
- Building Docker images → `iac-expert` loads automatically
- Writing Python type hints → `python-expert` loads automatically

### Manual Skill Invocation

You can also explicitly request a skill in your prompts:

```
Using the python-expert skill, help me implement async/await patterns
```

```
Invoke go-expert to review this HTTP handler for best practices
```

```
Use frontend-expert to optimize this React component for performance
```

### Skill Selection Guide

| If you need help with...                                   | Use this skill      |
| ---------------------------------------------------------- | ------------------- |
| React, Next.js, TypeScript, UI components, CSS             | `frontend-expert`   |
| Go development, testing, concurrency, HTTP handlers        | `go-expert`         |
| Cloud infrastructure code (Terraform, CDK, CloudFormation) | `iac-expert`        |
| Docker, containers, Kubernetes, orchestration              | `iac-expert`        |
| Python development, testing, type hints, async patterns    | `python-expert`     |

## Skills vs Instructions

| Aspect           | Instructions (`.github/instructions/`)  | Skills (`.claude/skills/`)         |
| ---------------- | --------------------------------------- | ---------------------------------- |
| **Location**     | `.github/instructions/*.instructions.md` | `.claude/skills/*/SKILL.md`        |
| **Loading**      | Always in context                       | On-demand (when relevant)          |
| **Purpose**      | Essential coding standards              | Deep domain expertise              |
| **Size**         | ~30 lines (minimal)                     | Comprehensive (100s of lines)      |
| **Context Cost** | Low (small, always loaded)              | Low (only loaded when needed)      |
| **Scope**        | Project-wide coding rules               | Specialized domain knowledge       |
| **Examples**     | `python.instructions.md`                | `python-expert/SKILL.md`           |

**Design Philosophy**: Instructions provide lightweight, always-applied standards. Skills provide comprehensive, on-demand expertise. This prevents context overflow while maintaining access to deep knowledge.

## Setup for VS Code

Skills work automatically with GitHub Copilot and Claude in VS Code:

1. **For GitHub Copilot**:
   - Ensure you have GitHub Copilot access via [Cortex Workflow](https://app.getcortexapp.com/admin/workflows/299)
   - Install the Copilot extension in VS Code
   - Skills are automatically detected from `.claude/skills/` directory

2. **For Claude/Anthropic**:
   - Skills are loaded from `.claude/skills/` and `.github/skills/` directories
   - No additional setup required

3. **Verify skills are loaded**:
   - Check VS Code's Output panel (View → Output → GitHub Copilot or Claude)
   - Skills should appear in the available capabilities list

## Adding New Skills

1. **Create skill directory**: `mkdir .claude/skills/my-skill`
2. **Create SKILL.md** with frontmatter:
   ```yaml
   ---
   name: my-skill
   description: Expert in [domain]. Use when working with [specific technologies/tasks].
   ---
   ```
3. **Follow the established structure**:
   - Title: `# My Skill Expert`
   - Introduction: "You are an Expert Software Engineer with deep specialization in..."
   - `## Core Expertise` with subsections (### Technologies, ### Best Practices, etc.)
   - `## Approach` with numbered workflow steps
   - Separator: `---`
   - Detailed sections with examples and code patterns
4. **Keep instructions minimal**: Update `.github/instructions/my.instructions.md` (20-30 lines)
5. **Update this README** with the new skill
6. **Test thoroughly** with various prompts and code scenarios

**Pattern Reference**: See [iac-expert/SKILL.md](./iac-expert/SKILL.md) or [frontend-expert/SKILL.md](./frontend-expert/SKILL.md) for structure examples.

## Related Resources

- [AI Coding Guide Repository](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide)
- [Expert Agents Repository](https://gitlab.com/vistaprint-org/vista-engineering/engineering-productivity/developer-productivity/experiments/expert-agents)
- [Vista AI Coding Configuration Guide](https://vistaprint.atlassian.net/wiki/spaces/NTEO/pages/4958650563)
- [Using Claude Skills with GitHub Copilot](https://vistaprint.atlassian.net/wiki/spaces/NTEO/pages/5255530829)

# Claude Skills for AI-Assisted Coding

This directory contains Claude Skills - specialized knowledge packs that provide on-demand expertise for specific development domains. Unlike instruction files that are always loaded into context, skills are invoked when needed, making them more context-efficient.

## Available Skills

| Skill                               | Command       | Use For                                                              |
| ----------------------------------- | ------------- | -------------------------------------------------------------------- |
| [IaC Expert](./iac-expert/SKILL.md) | `/iac-expert` | Terraform, AWS CDK, CloudFormation, Pulumi, Docker, containerization |

## Directory Structure

Each skill is stored in a folder with a `SKILL.md` file containing frontmatter metadata:

```
.claude/
└── skills/
    ├── README.md              # This file
    └── iac-expert/
        └── SKILL.md           # IaC & containerization expertise
```

## How to Use Skills

### In VS Code with GitHub Copilot

1. **Enable Claude Skills**: In VS Code settings, search for "skills" and enable "Claude skills for chat"
2. **Invoke a Skill**: Type the skill command followed by your question:

```
/iac-expert
Review this Terraform configuration for best practices
```

```
/security-expert
Review this authentication flow for vulnerabilities
```

### Skill Selection Guide

| If you need help with...                                   | Use this skill |
| ---------------------------------------------------------- | -------------- |
| Cloud infrastructure code (Terraform, CDK, CloudFormation) | `/iac-expert`  |
| Docker, containers, Kubernetes, orchestration              | `/iac-expert`  |

> More skills (security, frontend, quality) coming in future releases.

## Skills vs Instructions

| Aspect           | Instructions            | Skills                    |
| ---------------- | ----------------------- | ------------------------- |
| **Location**     | `.github/instructions/` | `.claude/skills/`         |
| **Loading**      | Always in context       | On-demand                 |
| **Purpose**      | Standards & rules       | Deep expertise            |
| **Context Cost** | High (always loaded)    | Low (loaded when invoked) |
| **Scope**        | Project/org-wide rules  | Domain-specific knowledge |

## Setup for GitHub Copilot

1. **Ensure you have GitHub Copilot access** - Request via [Cortex Workflow](https://app.getcortexapp.com/admin/workflows/299)
2. **Install the Copilot extension** in VS Code
3. **Enable Claude Skills**:
   - Open VS Code Settings (`Cmd+,` or `Ctrl+,`)
   - Search for "skills"
   - Enable **"Claude skills for chat"**
4. **Verify skills are loaded** - Ask Copilot: "What skills do you have access to?"

![Enable Skills in VS Code Settings](https://vistaprint.atlassian.net/wiki/download/attachments/5255530829/skills-setting.png)

## Adding New Skills

1. Create a new folder with a `SKILL.md` file (e.g., `my-skill/SKILL.md`)
2. Add frontmatter with `name` and `description`:
   ```yaml
   ---
   name: my-skill
   description: Brief description of when to use this skill
   ---
   ```
3. Follow the existing skill structure:
   - Core expertise areas
   - Best practices
   - Approach section
   - Examples and patterns
4. Update this README with the new skill
5. Test the skill with various prompts

## Related Resources

- [AI Coding Guide Repository](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide)
- [Expert Agents Repository](https://gitlab.com/vistaprint-org/vista-engineering/engineering-productivity/developer-productivity/experiments/expert-agents)
- [Vista AI Coding Configuration Guide](https://vistaprint.atlassian.net/wiki/spaces/NTEO/pages/4958650563)
- [Using Claude Skills with GitHub Copilot](https://vistaprint.atlassian.net/wiki/spaces/NTEO/pages/5255530829)

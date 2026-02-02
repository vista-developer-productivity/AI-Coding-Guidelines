# Contributing to Vista AI Coding Assistant

We welcome contributions to improve our AI-assisted development guidelines and coding standards. This repository serves as the central hub for Vista's coding conventions, AI coaching techniques, and development best practices.

## Who Can Contribute

- Anyone from Vista that has a passion for good AI Coding Assistant practices.

## What You Can Contribute

### Instructions (`.github/instructions/`)
Lightweight, always-loaded coding standards (~30-50 lines). Use for essential, language-specific rules.

### Skills (`.copilot/skills/`)
Comprehensive, on-demand expertise (400-850 lines). Use for deep domain knowledge and architectural patterns.

### Documentation
Improvements to guides like CODING_GUIDELINES.md, AI_RECENCY_BIAS_GUIDE.md, etc.

## Contributing Skills

Skills are the primary way to provide deep domain expertise to AI assistants. When contributing a new skill:

### When to Create a Skill vs. Update Instructions

| Create a Skill | Update Instructions |
|----------------|---------------------|
| Deep domain expertise needed | Basic coding standards |
| Comprehensive patterns (100+ lines) | Quick reference rules (~30 lines) |
| Framework/architecture guidance | Language syntax conventions |
| On-demand specialized knowledge | Always-needed project rules |

### Skill Creation Process

1. **Create the skill directory**:
   ```bash
   mkdir -p .copilot/skills/my-skill
   ```

2. **Create `SKILL.md`** with required frontmatter:
   ```yaml
   ---
   name: my-skill
   description: Expert in [domain]. Use when working with [specific technologies/tasks].
   ---
   ```

3. **Follow the established structure**:
   - **Title**: `# My Skill Expert`
   - **Introduction**: "You are an Expert Software Engineer with deep specialization in..."
   - **`## Core Expertise`**: Technologies, best practices subsections
   - **`## Approach`**: Numbered workflow steps
   - **Separator**: `---`
   - **Detailed sections**: Examples, code patterns, troubleshooting

4. **Update related instructions**: Keep `.github/instructions/*.instructions.md` minimal (~30 lines) with a reference to invoke the skill for deeper guidance.

5. **Update the skills README**: Add your skill to `.copilot/skills/README.md`.

6. **Test thoroughly**: Verify the skill is invoked correctly with various prompts.

### Skill Quality Guidelines

- **Be specific**: Provide concrete examples, not abstract principles
- **Show patterns**: Include code examples for recommended approaches
- **Avoid anti-patterns**: Describe what NOT to do without showing complete bad examples (see [NEGATION_AND_INSTRUCTION_CLARITY_GUIDE.md](NEGATION_AND_INSTRUCTION_CLARITY_GUIDE.md))
- **Cross-reference**: Link to related skills where appropriate
- **Keep it actionable**: Focus on guidance the AI can directly apply

## Contributing Instructions

When updating instruction files:

- Keep them lightweight (~30-50 lines maximum)
- Include a reference to invoke the corresponding skill for deeper guidance
- Follow positive framing (what TO do, not what NOT to do)
- Use measurable criteria instead of subjective terms

## Pull Request Guidelines

1. **Create a feature branch** from `main`
2. **Make focused changes** - one skill or instruction per PR when possible
3. **Test your changes** with AI assistants before submitting
4. **Update documentation** - README files, cross-references, etc.
5. **Request review** from the codeowners

## Questions?

Open an issue or reach out to the AI Engineering team.
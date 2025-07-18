# 🤖 Introduction

This guide empowers Vista engineers to configure AI coding assistants (e.g., GitHub Copilot, Claude, Cursor) to produce high-quality, maintainable, secure code aligned with [Vista Standards](https://vistaprint.atlassian.net/wiki/spaces/NTEO/pages/1418756120), the [Engineering Excellence](https://vistaprint.atlassian.net/wiki/spaces/VEORG/pages/3354165422) Maturity Model, [Project Validator](https://gitlab.com/vistaprint-org/vista-engineering/engineering-productivity/ci-cd/project-validator) from Deployment Platform team, [Curated Ops Best Practices](https://vistaprint.atlassian.net/wiki/spaces/~557058f22af46378a74f3693363f8cf90f1610/pages/3365836456) and other industry best practices (e.g., Twelve-Factor App, SemVer). It provides structured, opinionated guidance to boost AI suggestion acceptance (~18% currently) via instruction files like `.github/copilot-instructions.md`.

> **📝 Work in Progress**: This project is actively evolving. We welcome all feedback, suggestions, and contributions to improve these AI coding standards. Please see our [Contributing Guide](./CONTRIBUTING.md) to get involved!

## Why Copilot Instructions?

The `.github/copilot-instructions.md` or `.github/instructions/copilot.instructions.md` files guide AI tools by providing context on team workflows, tools, coding style, frameworks, and project-specific rules (e.g., via CONVENTIONS.md). Learn more in the VS Code Copilot [documentation](https://code.visualstudio.com/docs/copilot/copilot-customization).

## Setup Instructions

### For VS Code Users

VS Code automatically reads instruction files from your repository. Use the one-click install buttons in the table below for the easiest setup.

### For Other IDEs (Cursor, Claude, etc.)

If you're using a different IDE or AI assistant, you can still benefit from these instruction files:

1. **Download the instruction files** you need using the "Manual Download" links in the table below
2. **Copy the content** from the relevant `.instructions.md` files  
3. **Configure your IDE**:
   - **Cursor**: Paste content into your project's AI settings or include in prompts
   - **Claude/ChatGPT**: Include instruction content at the start of your conversations
   - **Other IDEs**: Check your IDE's documentation for custom AI configuration options

**Pro tip**: You can also copy these files to your project's `.github/instructions/` directory and reference them in your AI assistant's custom instructions.

## Customizing GitHub Copilot

In VSCode, GitHub Copilot can be tailored using Custom Instructions, Prompt Files, and Custom Chat Modes [VS Code Copilot Customization](https://code.visualstudio.com/docs/copilot/copilot-customization):

- [Custom Instructions](https://code.visualstudio.com/docs/copilot/copilot-customization#_custom-instructions): Use `copilot.instructions.md` files (e.g., `.github/copilot.instructions.md` / `.github/instructions/copilot.instructions.md` and `.github/instructions/*.instructions.md`) in repositories. Copilot automatically reads these by default to align suggestions with project-specific rules (e.g., TypeScript, React, Vista Standards).
`*.instructions.md` files add information to a given prompt when files matching the glob (via applyTo) are in the context window.
If a `CONVENTIONS.md` exists at the project’s top level, Copilot supplements instructions with its project-specific context (e.g., file structure, naming).
There are other customization files that are *not covered here* (yet):

  - [Prompt Files](https://code.visualstudio.com/docs/copilot/copilot-customization#_prompt-files-experimental): Use `.github/prompts/*.prompt.md` to take advantage of instruction files to reuse common guidelines and have task-specific instructions included in the prompt. For example, a security review prompt file can reference a custom instructions that describe general security practices, while also including specific instructions on how to report the findings of the review..
  - [Custom Chat Modes](https://code.visualstudio.com/docs/copilot/chat/chat-modes): Configure specialised response styles (e.g., concise, formal) via VS Code settings. VS Code comes with three built-in chat modes: `Ask`, `Edit`, and `Agent`. You can also define your own chat modes for specific scenarios, such as planning a new feature, or researching implementation options.

Many other IDEs also support similar setups.

## 💡 Contributing

We welcome contributions! Please see our [Contributing Guide](./CONTRIBUTING.md) for details on how to submit new instructions and prompts.

## 📋 Custom Instructions

Project-specific instructions to enhance GitHub Copilot's behavior for specific technologies and coding practices: (more will be added over time)

| Title | Description | VS Code Install | Manual Download |
| ----- | ----------- | --------------- | --------------- |
| [copilot.instructions.md](instructions/copilot.instructions.md) | Catchall guidelines for AI-assisted coding practices. | [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect?url=vscode%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fcopilot.instructions.md) [![Install in VS Code](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect?url=vscode-insiders%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fcopilot.instructions.md) | [📁 Download](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide/-/raw/main/.github/instructions/copilot.instructions.md?inline=false) |

---

| Title | Description | VS Code Install | Manual Download |
| ----- | ----------- | --------------- | --------------- |
| [Containerization & Docker Best Practices](instructions/containerization-docker-best-practices.instructions.md) | Comprehensive best practices for creating optimized, secure, and efficient Docker images and managing containers. Covers multi-stage builds, image layer optimization, security scanning, and runtime best practices. | [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect?url=vscode%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fcontainerization-docker-best-practices.instructions.md) [![Install in VS Code](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect?url=vscode-insiders%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fcontainerization-docker-best-practices.instructions.md) | [📁 Download](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide/-/raw/main/.github/instructions/containerization-docker-best-practices.instructions.md?inline=false) |
| [CSS/SCSS linting instructions](instructions/css.instructions.md) | Best practices for CSS/SCSS linting. | [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect?url=vscode%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fcss.instructions.md) [![Install in VS Code](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect?url=vscode-insiders%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fcss.instructions.md) | [📁 Download](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide/-/raw/main/.github/instructions/css.instructions.md?inline=false) |
| [Go Development Instructions](instructions/go.instructions.md) | Instructions for writing Go code following idiomatic Go practices and community standards. | [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect?url=vscode%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fgo.instructions.md) [![Install in VS Code](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect?url=vscode-insiders%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fgo.instructions.md) | [📁 Download](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide/-/raw/main/.github/instructions/go.instructions.md?inline=false) |
| [Java Development](instructions/java.instructions.md) | Guidelines for building Java base applications. | [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect?url=vscode%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fjava.instructions.md) [![Install in VS Code](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect?url=vscode-insiders%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fjava.instructions.md) | [📁 Download](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide/-/raw/main/.github/instructions/java.instructions.md?inline=false) |
| [Markdown Content Rules](instructions/markdown.instructions.md) | Documentation and content creation standards. | [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect?url=vscode%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fmarkdown.instructions.md) [![Install in VS Code](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect?url=vscode-insiders%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fmarkdown.instructions.md) | [📁 Download](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide/-/raw/main/.github/instructions/markdown.instructions.md?inline=false) |
| [Python Coding Standards](instructions/python.instructions.md) | Python coding conventions and guidelines. | [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect?url=vscode%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fpython.instructions.md) [![Install in VS Code](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect?url=vscode-insiders%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fpython.instructions.md) | [📁 Download](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide/-/raw/main/.github/instructions/python.instructions.md?inline=false) |
| [TypeScript and JavaScript Coding Standards](instructions/ts.instructions.md) | Best practices for writing JavaScript/TypeScript code. | [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect?url=vscode%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fts.instructions.md) [![Install in VS Code](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect?url=vscode-insiders%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fvista-developer-productivity%2FAI-Coding-Guidelines%2Fmain%2F.github%2Finstructions%2Fts.instructions.md) | [📁 Download](https://gitlab.com/vistaprint-org/ai-engineering/ai-coding-guide/-/raw/main/.github/instructions/ts.instructions.md?inline=false) |

---

![Alt text](/media/demo.gif "Adding instructions to VS Code")
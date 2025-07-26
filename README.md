# AI Coding Guide

This repository provides guidance for working effectively with AI-assisted development tools across our organization. As AI coding assistants become integral to our development workflow, it's essential to understand how to guide these tools to produce code that aligns with our standards and engineering best practices.

## Getting Started

**CODING_GUIDELINES.md** is your primary resource, containing practical techniques for instructing your IDE agent to behave correctly. LLMs are often very optimistic, and as an Engineer it is your responsibility to guide and coach AI agents to align with our internal guidelines and general software engineering best practices.

### Understanding AI Limitations

For engineers who want to understand *why* certain coaching techniques are necessary, we've also included **AI_RECENCY_BIAS_GUIDE.md**. This technical guide explains the architectural reasons why AI assistants sometimes "forget" earlier instructions or drift from established patterns during long conversations. Reading this guide will help you:

- Recognize when AI is exhibiting recency bias (favoring recent information over earlier context)
- Understand why file-based documentation is more reliable than conversational instructions
- Know when to reset a conversation versus continuing with corrective techniques

**Start with CODING_GUIDELINES.md for practical techniques, then read AI_RECENCY_BIAS_GUIDE.md when you need to understand the underlying science.**

We encourage all teams to share their templates in this directory for greater knowledge sharing and a more community-driven approach to ensuring our AI assisted tools work in the most effective way possible across the organization. When using VSCode, follow the following links on how to add more specific instructions to your IDE. 
 The default behavior is a file in `.github/copilot-instructions.md` 
 
 Links for reference : 
 
 https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot?tool=vscode
 
 https://code.visualstudio.com/docs/copilot/copilot-customization#:~:text=In%20the%20Chat%20view%2C%20select,to%20specific%20files%20or%20folders.
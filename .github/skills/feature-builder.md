---
name: feature-builder
description: Use this skill when asked to build a feature from a Jira ticket. This skill provides guidance on creating a comprehensive implementation plan, building necessary infrastructure, writing code, and ensuring the feature is production-ready.
---

# Feature Builder

You are an expert Solutions Architect. Your goal is to build complete, production-ready features.

## Project contributor

As a contributor to the project, all changes to code must follow the guidance documented in the repository. Always remember, you are a guest in someone else's repository. Respect the project's contribution process (like branch naming or commit formats).

Most every project has a set of contribution guidelines everyone needs to follow when contributing to a project. Explore the project to determine if there's any guidance, read through what you find and apply the guidance related to contribution workflow: branch naming, commit message format, MR templates, and similar process steps.

If no guidance is found, or doesn't provide guidance on certain topics, then use the following as a foundation for creating a quality contribution.

### Branch

Before performing any commits, ensure a branch has been created for the work. Apply branch naming conventions from the repository's documentation (prefixes like feature or chore, username patterns, etc.). This branch must never be main, or the default branch, but should be a branch created specifically for the changes taking place.

If there are no naming conventions for branches, create a new one with the following naming:

```bash
  git checkout -b <ticket-number>-<short-description>
```

### Commits

When committing changes, logically group the changes together. Create short commit messages for each group, following any guidance in the repository.Commit the grouped code to the branch.

### Merge Requests

When creating a merge request, use existing templates in the repository if any exist as formatting structure, fill in their headings and sections.

If no template is provided, use the this PR template:

- Summary
- Background
- Changes
- Testing
- Additional Notes

Reference the Jira ticket in the MR. Use the Closes #NUMBER syntax to enable auto-closing of the issue.

## Pre-requisites

This skill works better if the following MCP servers are available and responsive:

- **Atlasian** - Jira and Confluence tools
- **Gitlab** - Gitlab tools

## Instructions

Make sure you remind to the user that the enviroment is set up and ready to go before starting to execute the plan (AWS credentials, JWT tokens, Gitlab access, Jira access, etc.).

**You always start in planning mode**. Your initial task is to generate an implementation plan for the feature detailed in the ticket.
**The plan must always be based on the stages detailed below**.

Your final TODO list must ALWAYS follow this workflow, in strict order, and without skipping any step:

1. Understand the requirements --> 2. Set up the branch --> 3. Generate iac (only if needed) --> 4. **Deploy infrastructure (push code, it will be deployed through a pipeline in Gitlab)** (only if needed) --> 5. Implement the feature --> 6. Run scripts (only if needed) --> 7. **Local run** --> 8. Push and open MR --> 9. Quality gates

> **Note**: Stage numbers are fixed. When a stage is skipped (e.g., no IaC or no scripts needed), proceed to the next applicable stage — do not renumber.

**Always Double check with the user that the plan is complete and satisfactory before you start executing it.**

## Workflow

Here a decsription of the above stages, with instructions on how to execute each of them.

### Stage 1 — Understand the requirements

1. Read the Jira ticket in full. Extract:

- The user-facing or system behavior being added
- Acceptance criteria
- Any linked tickets, designs, or specs

2. Identify ambiguities. If anything is unclear about scope, data model, API contract, or expected behavior — ask now, not mid-implementation.
3. Summarize your understanding back in one paragraph before continuing. This acts as your implementation contract.

### Stage 2 — Set up the branch

1. Pull the latest `main` (or the project's default branch).
2. Create a feature branch.
3. Confirm the branch is clean and tracking correctly before writing any code.

### Stage 3 — Build infrastructure (AWS / IaC)

**First, decide**: Does this feature require new or modified AWS infrastructure?

Ask yourself:

- New storage (S3, DynamoDB, RDS)?
- New compute (Lambda, ECS, EC2)?
- New queues, topics, or event buses (SQS, SNS, EventBridge)?
- New networking, secrets, or IAM roles?

In case that new infrastructure is needed:

0. **Project's folder for IaC**:

- Identify where the project's infrastructure-as-code lives. This might be a `iac/` directory, a `cdk/` folder, or something similar. This is where you will add new code for the infrastructure.

1. **Gather context**

- Use available MCP servers and skills that might help you creating the infrastructure, like AWS IaC MCP Server or iac-expert skill.
- Review existing IaC for patterns, naming conventions, and module structure to follow

2. **Design the infrastructure**

- Describe the resources you plan to create and why
- Note any dependencies between new and existing resources

3. **Implement the IaC**

- Resource names should be short and clear, following project conventions or specific requirements set at the JIRA ticket
- Use variables and outputs correctly; avoid hardcoding environment-specific values

4. **Commit changes**

- Use this message format: `Add infrastructure for <feature>`

### Stage 4 — Deploy infrastructure (AWS / IaC)

**Do not deploy the infrastructure locally. Do not ask the user to deploy it manually. It will be deployed as part of the CI/CD pipeline in the next steps**.

1. **Push to origin**

```
  git push origin <branch>
```

2. **Wait for the pipeline**

- Do not ask the user to check the CI/CD pipeline. Instead, monitor it using the corresponding tools until it completes, this will deploy the new infrastructure to AWS
- If it fails: read the logs, fix the issue, re-push

3. **Verify in AWS**

- Confirm the expected resources exist and are in the correct state
- Check IAM permissions, connectivity, and configuration

### Stage 5 — Implement the feature

Before starting to write code make sure that, if infrastructure was needed, stages 3 and 4 were completed successfully. The infrastructure must be in place before you can implement the feature that depends on it.

1. **Write the code**

- Follow the project's existing patterns, naming conventions, and architecture
- Keep changes focused — don't refactor unrelated code in the same branch

2. **Write unit tests**

- Cover the happy path and key edge cases
- Run the test suite and confirm all tests pass

3. **Lint and type-check**

- Run linters and type checkers configured for the project. Instructions might be in `README.md` or `DEVELOPER.md`.
- Fix all errors before proceeding. Warnings should be reviewed and addressed where practical.

4. **Update documentation**

- Add a section to `README.md` describing the new feature: what it does, how to use it, and any configuration needed

### Stage 6 — Run scripts

**First, decide**: Does this feature require running any scripts before the application can be used locally?

Skip this stage unless the feature requires data seeding, migrations, code generation, or environment setup to function locally.

1. **Identify the scripts to run**

- Locate relevant files in scripts/, migrations/, or as specified in the project documentation.

2. **Review before executing**

- Read the script to understand its impact. Ensure all target resources (databases, buckets) and environment variables are ready before running.

3. **Run the script**

- Run the script using project conventions (e.g., npm run migrate). If it fails, resolve the error and re-run until successful.

4. **Verify the outcome**

- Perform spot-checks (query data, list files) to confirm the script achieved the intended results.

5. **Document the script usage**

- If the script is new or updated, add its purpose, prerequisites, and usage examples to the README.md.

### Stage 7 — Local run

**This stage is crucial**. Do not make assumptions about the feature working without actually running it end-to-end locally. This is your chance to catch any issues before pushing code or opening an MR.

1. **Run locally**

- Find instructions for running the application locally (they should be specified at `README.md`, `AGENTS.md`, `DEVELOPER.md` or similar). If none exist, ask for guidance.
- Run the application end-to-end following the instructions.
- Verify the new feature works as expected and does not break existing functionality.

### Stage 8 — Push and open MR

Make sure you performed a local run and verified the feature works before pushing code. This is critical to avoid broken builds and failed pipelines.
Execute the following steps in strict order and one by one, don't skip any step.

1. **Commit**
2. **Push**

```
  git push origin <branch>
```

3. **Open or update the Merge Request**

### Stage 9 — Quality gates

Before marking work as done, double check that:

- [ ] All acceptance criteria from the ticket are met
- [ ] Unit tests pass
- [ ] No lint or type errors
- [ ] Feature works end-to-end locally
- [ ] Infrastructure is deployed and verified (if applicable)
- [ ] README is updated
- [ ] MR is open with a meaningful description

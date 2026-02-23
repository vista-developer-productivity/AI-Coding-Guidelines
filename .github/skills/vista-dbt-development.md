---
name: vista-dbt-development
description: Establishes Vista dbt conventions and Snowflake MCP usage for analytics engineering tasks, while deferring generic dbt workflows to existing dbt skills.
---

# Vista dbt Development

Use this skill when working on dbt models, tests, or documentation in Vista projects.

## Always Check First

- `CONVENTIONS.md` for project-specific overrides.

## Project Architecture

- Use layered modeling: staging, intermediate, and core layers.
- Source isolation: only staging models use `source()`, downstream models use `ref()`.

## Dimensional Modeling

- Favor star/snowflake schemas: fact tables for measurable events, dimension tables for context.
- Ensure clear relationships between facts and dimensions.

## Documentation and Metadata

- Model docs must be business-friendly and comprehensive.
- Set `docs.show: true`.
- Persist docs with `config.persist_docs.relation: true` and `config.persist_docs.columns: true`.
- Declare `data_type` for every column.
- Include relationships and sample values for complex fields when helpful.

## Contracts and Constraints

- Enable `config.contract.enforced: true` on production models.
- Use constraints for `primary_key`, `not_null`, and `unique`.
- Set `warn_unenforced: false` where Snowflake does not enforce constraints.

## Project-Level Configuration

- `+persist_docs` must be enabled in `dbt_project.yml`.
- `+copy_grants: true` to preserve permissions on rebuilds.
- Add `+query_tag` metadata for lineage and usage tracking.

## Testing Requirements

- Primary keys: `not_null` and `unique`.
- Foreign keys: `relationships` tests to the parent model.
- Required fields: `not_null`.
- Categorical fields: `accepted_values` (warn for soft checks).
- Custom rules: `check` constraints or custom tests when needed.

## Snowflake MCP Usage

Use the Snowflake MCP server for discovery and validation when it reduces manual SQL work.

## Defer to Specialized dbt Skills

Use these skills for deeper dbt workflows and command guidance:

- `adding-dbt-unit-test`
- `fetching-dbt-docs`
- `running-dbt-commands`
- `troubleshooting-dbt-job-errors`
- `using-dbt-for-analytics-engineering`

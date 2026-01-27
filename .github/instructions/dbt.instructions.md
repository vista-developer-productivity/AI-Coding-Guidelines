---
description: "dbt development guidelines for building and maintaining dbt projects"
applyTo: "dbt/**, **.dbt_project.yml"
---

# dbt Development Guidelines [v1.0]

Minimal standards for dbt projects. For comprehensive guidance, invoke the `data-expert` skill.

**Note**: Check for project-specific conventions in `CONVENTIONS.md` files, which may override these defaults.

## Core Standards

### Project Organization

- Use layered architecture: staging → intermediate → core
- Staging models only reference `{{ source() }}`
- All other models use `{{ ref() }}`

### Naming Conventions

- Staging: `stg_{source_system}__{table}.sql`
- Intermediate: `int_{domain}__{description}.sql`
- Dimensions: `dim_{entity}.sql`
- Facts: `fact_{event}.sql`
- Use `snake_case`, double underscores (`__`) for separation

### Documentation

- Every model must have a corresponding YAML file
- Include business-friendly descriptions
- Document column data types and constraints
- Enable `persist_docs` for metadata

### Testing

- Use `unique` and `not_null` for primary keys
- Use `relationships` for foreign keys
- Use `accepted_values` for categorical data
- Define tests in YAML files

### Code Quality

- Use CTEs for complex logic
- Use descriptive names for CTEs and aliases
- Comment complex business logic
- Format SQL consistently (indentation, comma placement)

### Materialization

- **ephemeral**: Simple transformations
- **view**: Light transformations, infrequent queries
- **table**: Complex models, frequent queries
- **incremental**: Large datasets, append patterns

## Comprehensive Guidance

For detailed dbt patterns, semantic layer optimization, testing strategies, and advanced features, invoke the **`data-expert` skill**.

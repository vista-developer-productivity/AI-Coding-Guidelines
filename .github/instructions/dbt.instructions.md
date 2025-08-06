---
description: 'dbt development guidelines for building and maintaining dbt projects'
applyTo: 'dbt/**, **.dbt_project.yml'
---

# dbt Development Guidelines

This guide outlines general best practices for dbt development. Teams have autonomy to adapt these guidelines to their specific needs and conventions.

**Note**: Check for project-specific conventions in `CONVENTIONS.md` files, which may override these defaults.

## Project Architecture

### Layered Approach

Organize your dbt project using a layered architecture to ensure clear data lineage and maintainability:

- **Staging Layer**: Raw source data cleaning and standardization
- **Intermediate Layer**: Business logic and complex transformations
- **Core Layer**: Final models for consumption (dimensions, facts, marts)

### Source Isolation Principle

- Only staging models should reference sources directly using `{{ source() }}` function
- Downstream models should reference staging models using `{{ ref() }}` function
- This creates clear boundaries and makes impact analysis easier

Example:

```sql
-- Good: Staging model references source
SELECT * FROM {{ source('raw_data', 'customers') }}

-- Good: Core model references staging model
SELECT * FROM {{ ref('stg_customers') }}

-- Avoid: Core model referencing source directly
SELECT * FROM {{ source('raw_data', 'customers') }}
```

## Model Organization

### Preferred Directory Structure

**Standard Organization** (unless overridden in project's `CONVENTIONS.md`):

```
models/
├── staging/
│   ├── {source_system}/
│   │   ├── stg_{source_system}__{table}.sql
│   │   └── stg_{source_system}__{table}.yml
│   └── _sources.yml
├── intermediate/
│   ├── int_{domain}__{description}.sql
│   └── int_{domain}__{description}.yml
├── core/
│   ├── dimensions/
│   │   ├── dim_{entity}.sql
│   │   └── dim_{entity}.yml
│   ├── facts/
│   │   ├── fact_{entity}.sql
│   │   └── fact_{entity}.yml
│   └── marts/
│       ├── mart_{domain}__{description}.sql
│       └── mart_{domain}__{description}.yml
└── semantic/
    └── {entity}.yml
```

**Organization Principles:**

- **Layer-first**: Primary organization by data transformation layer
- **Source grouping**: Staging models grouped by source system
- **Domain grouping**: Core models can be grouped by business domain if needed
- **1:1 Documentation**: Every SQL model file has a corresponding YAML documentation file

### Preferred Naming Conventions

**File Naming Patterns** (unless overridden in project's `CONVENTIONS.md`):

| Model Type | SQL File | YAML File | Examples |
|------------|----------|-----------|----------|
| **Staging** | `stg_{source_system}__{table}.sql` | `stg_{source_system}__{table}.yml` | `stg_employee__blueprint_employee.*` |
| **Intermediate** | `int_{domain}__{description}.sql` | `int_{domain}__{description}.yml` | `int_finance__monthly_revenue.*` |
| **Dimensions** | `dim_{entity}.sql` | `dim_{entity}.yml` | `dim_customer.*`, `dim_product.*` |
| **Facts** | `fact_{process/event}.sql` | `fact_{process/event}.yml` | `fact_orders.*`, `fact_page_views.*` |
| **Marts** | `mart_{domain}__{description}.sql` | `mart_{domain}__{description}.yml` | `mart_finance__revenue_summary.*` |
| **Sources** | N/A | `_sources.yml` | Located in staging directory |

**General Naming Rules:**

- Use `snake_case` for all file and model names
- Use double underscores (`__`) to separate major components
- Use single underscores (`_`) within components
- Keep names descriptive but concise
- Avoid abbreviations unless they're widely understood in your domain
- Every SQL model must have a corresponding YAML file with the same base name

## Documentation Standards

### Comprehensive Model Documentation

Every model should have corresponding YAML documentation that serves both technical and business purposes. Use progressive levels of detail based on model importance:

#### Model Documentation Examples

Every model should have comprehensive YAML documentation that demonstrates progressive levels of detail and business context:

```yaml
models:
  - name: dim_customer
    description: |
      Customer dimension table containing comprehensive customer information including
      demographics, contact details, and account status. This table serves as the
      primary source of truth for all customer-related analytics and reporting.

      Business Context: Used by Marketing, Sales, and Customer Success teams for
      segmentation, campaign targeting, and customer lifecycle analysis.
    docs:
      show: true
    config:
      contract:
        enforced: true
      persist_docs:
        relation: true
        columns: true
    columns:
      # Standard primary key documentation
      - name: customer_id
        description: Unique identifier for each customer account
        data_type: varchar
        constraints:
          - type: primary_key
            warn_unenforced: false
          - type: not_null
        data_tests:
          - not_null
          - unique
      
      # Business-friendly documentation with usage context
      - name: customer_lifetime_value
        description: |
          Total predicted revenue from this customer over their entire relationship
          with the company. Calculated using purchase history, engagement metrics,
          and predictive modeling. Used by Sales for account prioritization and
          Marketing for campaign targeting.
        data_type: number(10,2)
        
      - name: acquisition_channel
        description: |
          The marketing channel through which we first acquired this customer.
          Common values include 'paid_search', 'social_media', 'direct', 'referral'.
          Critical for marketing attribution and campaign ROI analysis.
        data_type: varchar
        
      # Categorical field with constraints
      - name: customer_status
        description: Current status of the customer account
        data_type: varchar
        constraints:
          - type: not_null
        data_tests:
          - accepted_values:
              values: ["active", "inactive", "suspended"]
          - not_null
```

### Model Configuration Parameters

- `description`: Detailed business description including purpose, usage, and business context
- `docs.show`: Enable documentation visibility (recommended: true)
- `config.contract.enforced`: Enable schema contract enforcement for critical models
- `config.persist_docs.relation`: Persist model descriptions to database metadata
- `config.persist_docs.columns`: Persist column descriptions to database metadata

### Data Type and Constraint Management

Always declare data types explicitly and use constraints for data quality:

#### Available Constraint Types

- `primary_key`: Defines primary key constraint (may be metadata-only in some databases)
- `not_null`: Enforces non-null values
- `unique`: Enforces unique values
- `check`: Custom validation rules with expressions
- `warn_unenforced`: Set to false to suppress warnings for metadata-only constraints

### Global Configuration (dbt_project.yml)

Configure documentation and metadata persistence at the project level:

```yaml
models:
  +persist_docs:
    relation: true # Persist model descriptions
    columns: true # Persist column descriptions
  +copy_grants: true # Preserve permissions on object recreation
  +query_tag: # Add governance metadata
    domain_name: "{{ var('domain_name', 'unknown') }}"
    product_team: "{{ var('product_team', 'unknown') }}"
    data_product: "{{ var('data_product', 'unknown') }}"
```

### Contract Enforcement

For critical business models, enable contract enforcement:

```yaml
models:
  - name: dim_customer
    config:
      contract:
        enforced: true
    columns:
      - name: customer_id
        data_type: varchar
        constraints:
          - type: not_null
          - type: primary_key
```

### Documentation Requirements

- **Model descriptions**: Explain the business purpose and use case
- **Column descriptions**: Document what each field represents using business language
- **Business context**: Include how the model should be used and by whom
- **Data lineage**: Document upstream dependencies and transformations
- **Sample values**: Include representative examples for complex fields
- **Relationships**: Document foreign key relationships clearly

### Benefits of Rich Documentation

- **Documentation Persistence**: Model and column descriptions become database metadata
- **Semantic Layer Integration**: Rich metadata powers natural language queries
- **Governance Tracking**: Query tags enable usage monitoring and data lineage
- **Permission Management**: Copy grants maintain access controls during deployments

## Testing and Data Quality Strategy

### Comprehensive Testing Approach

Implement data quality tests at multiple levels to ensure reliability:

#### Basic Data Quality Tests

- `unique` and `not_null` for primary keys
- `relationships` for foreign keys
- `accepted_values` for categorical data
- `check` constraints for custom validation rules

#### Relationship Testing

Document and test foreign key relationships clearly:

```yaml
# In fact table
columns:
  - name: customer_id
    description: References the customer dimension for customer details
    data_type: varchar
    constraints:
      - type: not_null
    data_tests:
      - relationships:
          to: ref('dim_customer')
          field: customer_id
```

### Testing Placement and Organization

- Define tests in model YAML files for maintainability
- Consider custom tests for complex business rules
- Test staging models for source data quality
- Test core models for business logic accuracy
- Use constraints and contracts for schema enforcement

### Data Quality Monitoring

- Implement appropriate testing for data reliability
- Monitor test results and failures
- Document data quality assumptions and constraints
- Establish data quality SLAs where applicable
- Use constraints and contracts for schema enforcement

## Code Quality

### SQL Formatting

While style preferences vary, consider these readability practices:

- Use consistent indentation
- Place commas at the end of lines or beginning (be consistent)
- Use meaningful aliases
- Format complex queries with appropriate line breaks

### CTEs and Modularity

- Break complex logic into readable CTEs
- Use descriptive CTE names
- Comment complex business logic
- Keep models focused on single responsibility

Example:

```sql
WITH base_customers AS (
    SELECT * FROM {{ ref('stg_customers') }}
),

active_customers AS (
    SELECT *
    FROM base_customers
    WHERE status = 'active'
),

final AS (
    SELECT
        customer_id,
        email,
        created_at,
        -- Business logic here
        CASE WHEN tenure_days > 365 THEN 'veteran' ELSE 'new' END AS customer_segment
    FROM active_customers
)

SELECT * FROM final
```

## Development Workflow

### Environment Management

- Use separate profiles for development, staging, and production
- Leverage environment variables for configuration
- Test changes in development environment before promoting

### Version Control

- Include all dbt project files in version control
- Use meaningful commit messages
- Consider branching strategies for collaborative development

## Package Management

### Common Useful Packages

Consider these widely-adopted packages:

- **dbt-utils**: Essential macros and tests
- **dbt-expectations**: Advanced data quality tests
- **codegen**: Code generation helpers

### Package Management Best Practices

- Pin package versions for reproducibility
- Document why packages are included
- Regular updates and dependency management

## Performance Considerations

### Materialization Strategies

Choose appropriate materializations based on use case:

- **ephemeral**: For simple transformations (staging models)
- **view**: For models with light transformations
- **table**: For complex models or frequently queried data
- **incremental**: For large datasets with append-only patterns

### Optimization Tips

- Use appropriate filters and WHERE clauses
- Consider partitioning and clustering for large datasets
- Monitor model run times and optimize bottlenecks
- Use `dbt run --select` for targeted development

## Semantic Model Best Practices

Modern data platforms increasingly support semantic layers and natural language querying. Following these practices ensures your dbt models are optimized for semantic model generation and business intelligence tools.

### Semantic Layer Documentation Guidelines

#### Writing Effective Descriptions

- **Use Business Language**: Avoid technical jargon, use terms business users understand
- **Include Context**: Explain how the data should be used and by whom
- **Provide Examples**: Include sample values or common use cases
- **Document Relationships**: Explain how tables connect and relate to each other

### Semantic Model Optimization

#### Preparing for Semantic Layer Integration

1. **Consistent Naming**: Use business-friendly names that match how users think about data
2. **Comprehensive Relationships**: Document all foreign key relationships clearly  
3. **Sample Values**: Include representative sample values in descriptions
4. **Synonyms and Alternatives**: Consider alternative names users might use for concepts

#### Relationship Documentation Best Practices

Ensure foreign key relationships are clearly documented:

```yaml
# In dimension table
columns:
  - name: customer_id
    description: Unique customer identifier, links to fact tables
    data_type: varchar
    constraints:
      - type: primary_key
```

### Implementation Guidelines

#### Model Development Checklist

When creating or updating models for semantic layer compatibility:

- [ ] **Comprehensive YAML documentation** with business context
- [ ] **Explicit data type declarations** for all columns
- [ ] **Business-friendly descriptions** using domain language
- [ ] **Constraint definitions** for data quality and relationships
- [ ] **Contract enforcement** enabled for critical models
- [ ] **Persist docs configuration** enabled
- [ ] **Relationship tests** implemented for foreign keys
- [ ] **Sample values or examples** included in complex field descriptions

#### Semantic Model Preparation Strategies

- **Business Alignment**: Write descriptions from the business user's perspective
- **Comprehensive Coverage**: Document all columns that business users might query
- **Clear Relationships**: Make table relationships explicit and well-documented
- **Consistent Terminology**: Use consistent business terms across related models
- **Quality Assurance**: Implement robust testing to ensure semantic accuracy

### Advanced Features

#### Query Governance

Implement query tagging for governance and monitoring:

```yaml
# In dbt_project.yml
models:
  staging:
    +query_tag:
      layer: "staging"
      purpose: "data_cleaning"
  core:
    +query_tag:
      layer: "core"
      purpose: "business_logic"
```

### Benefits of Semantic Optimization

- **Automated Documentation**: Metadata flows automatically to semantic layer tools
- **Natural Language Queries**: Rich metadata enables AI-powered data querying
- **Self-Service Analytics**: Business users can explore data with confidence
- **Improved Governance**: Clear lineage and documentation improve data stewardship
- **Reduced Support Burden**: Well-documented models reduce data team support requests

### Common Pitfalls and Solutions

#### Relationship Constraints

- **Issue**: Semantic models may require referenced columns to have unique constraints
- **Solution**: Ensure foreign key references point to columns with primary_key or unique constraints

#### Business Language Gaps

- **Issue**: Technical column names don't match business terminology
- **Solution**: Use descriptive aliases and comprehensive descriptions with business synonyms

#### Incomplete Documentation

- **Issue**: Missing descriptions prevent effective semantic layer generation
- **Solution**: Implement documentation reviews and require descriptions for all production models

## Data Governance

### Metadata Management

- Maintain comprehensive model documentation following Documentation Standards
- Document data lineage and business rules
- Include data owners and stewards information
- Regular documentation reviews and updates

### Data Quality

- Follow comprehensive testing strategy outlined in Testing and Data Quality section
- Monitor test results and failures systematically
- Establish data quality SLAs where applicable

## Development Best Practices

### Model Development

- Start simple and iterate
- Write models that are easy to understand and maintain
- Consider the end user when designing final models
- Validate outputs against business expectations

### Collaboration

- Use consistent conventions within your team
- Document team-specific standards
- Regular code reviews for knowledge sharing
- Maintain clear model ownership and responsibilities

## Deployment Considerations

### Production Readiness

- Ensure all models have appropriate tests
- Validate model performance in production-like environment
- Plan for monitoring and alerting
- Document deployment procedures and rollback plans

### CI/CD Integration

- Automate testing in deployment pipeline
- Consider staged deployments (dev → staging → production)
- Monitor data freshness and quality post-deployment

---

## Project-Specific Overrides

Always check for a `CONVENTIONS.md` file in your dbt project root, which may contain:

- Project-specific naming conventions
- Custom folder structures
- Domain-specific terminology
- Team-agreed deviations from these standards

When project conventions exist, they take precedence over these general guidelines.

# Performance Optimization

## Application Performance (Code-Level)

- **Code Splitting**: Implement lazy loading for components and routes
- **Bundle Analysis**: Monitor and optimize bundle sizes regularly (webpack-bundle-analyzer or similar)
- **Database Optimization**: Use efficient queries and proper indexing
- **Async Processing**: Use background jobs for heavy operations
- **Image Optimization**: Compress and serve modern formats (WebP, AVIF) with fallbacks
- **Critical Path**: Optimize critical rendering path

### TypeScript/JavaScript Specific

- Use tree-shaking friendly imports (named imports, not `import *`)
- Avoid importing entire libraries when only using specific functions
- Use barrel exports judiciously to avoid circular dependencies
- Set bundle size budgets and fail builds if exceeded
- Use `React.lazy()` and dynamic imports for heavy components

## Infrastructure Performance (Deployment-Level)

- **Caching Strategy**: Implement caching at multiple levels (browser, CDN, application, database)
- **CDN Usage**: Serve static assets from CDN
- **Connection Pooling**: Use connection pooling for database access
- **Rate Limiting**: Implement tunable rate limiting mechanisms to prevent abuse

## Data & Storage Performance

### Database Selection Philosophy

> **Maintenance Mantra.** Start with the most managed and operationally simple service, moving to more specialized options only as needed. Keep your architecture lean and minimise operational overhead and cognitive load. The more time spent maintaining databases, the less time for developing your application. Don't only optimise for cost, factor in operational burden, engineering time, and your team's familiarity with the technology.

### Three Decision Lenses

| Lens | Question | Guidance |
|------|----------|----------|
| **Purpose** | SQL or NoSQL? | Structured data with relationships and ACID needs → SQL. Unstructured, semi-structured, or ephemeral data → NoSQL. |
| **Scope** | Scale and query patterns? | Predictable key-value lookups at massive scale → NoSQL. Complex ad-hoc queries, joins, reporting → SQL. |
| **Maturity** | Prototype or production? | Early/exploratory stages → start with SQL (PostgreSQL). It allows ad-hoc queries and schema evolution without committing to access patterns upfront. |

### Technology Recommendations (per Backend Tech Radar)

| Technology | Status | Use For | Key Caveat |
|-----------|--------|---------|------------|
| **Redis / Valkey** | ✅ Adopt | Caching, sessions, rate limiting, ephemeral data | Not a primary datastore, use as cache layer in front of DB |
| **Aurora PostgreSQL** | ✅ Adopt | Relational data, ACID, complex queries, systems of record | **Default choice.** Combine with Redis for hot data. Use Aurora for bursty workloads |
| **DynamoDB** | ✅ Adopt | High-throughput key-value, simple lookups | Requires upfront access pattern design; complex queries are expensive |
| **DocumentDB (MongoDB)** | ✅ Adopt | Flexible schemas, hierarchical documents, rapid iteration | Avoid if data is highly relational; choose shard key carefully |
| **OpenSearch** | ✅ Adopt | Full-text search, log analytics, vector search | |
| **S3** | ✅ Adopt | Object storage, static assets, data lake | |
| MongoDB Atlas | ❌ Hold |, | Use DocumentDB or Aurora PostgreSQL instead |

## Profiling & Measurement

- Profile before optimizing, measure, don't guess
- Use APM tools (New Relic, OpenTelemetry) to identify bottlenecks
- Benchmark critical code paths
- Focus on algorithmic improvements first, micro-optimizations last

# Database Architecture, Schema Design & ORM Modeling

## Purpose
Governs relational and document database modeling, index optimization, migration management, and high-performance ORM access (Prisma / Drizzle ORM) while preventing N+1 queries.

## When To Use
- When defining database schemas, tables, relationships, and indexes.
- When generating and running database migrations.
- When querying data, optimizing slow queries, and designing multi-table transactions.

## When Not To Use
- When storing ephemeral in-memory cache data (use Redis or memory store).

## Core Principles
1. **Relational Integrity by Default**: Enforce foreign keys, unique constraints, and check constraints at the database level—never rely solely on application-level checks.
2. **Explicit Indexing**: Every column used in `WHERE`, `JOIN`, `ORDER BY`, or `GROUP BY` clauses on high-volume tables must have an index.
3. **Zero Data Loss Migrations**: Database migrations must be backward-compatible and safe for zero-downtime deployments.

## Rules
- **Rule 1 (ORM Selection Standard)**:
  - Use **Drizzle ORM** for maximum serverless performance, zero cold-start penalty, and explicit SQL query control.
  - Use **Prisma ORM** for enterprise codebases requiring comprehensive auto-generated relationship tooling and mature ecosystem integration.
- **Rule 2 (Primary & Foreign Key Standards)**:
  - Primary keys must use distributed unique IDs: `cuid2`, `uuidv7`, or `ulid`. Never expose sequential auto-incrementing integers (`1, 2, 3`) in public IDs (prevents enumeration attacks).
  - Foreign keys must declare explicit `onDelete` actions (`Cascade`, `Restrict`, or `SetNull`).
- **Rule 3 (Mandatory Indexing Rules)**:
  - Primary keys: Automatically indexed.
  - Foreign key columns (`userId`, `orgId`): **Must be indexed**.
  - Compound query filters: Create multi-column indexes reflecting exact query order:
    ```sql
    CREATE INDEX idx_orders_tenant_status_created ON orders (tenant_id, status, created_at DESC);
    ```
- **Rule 4 (N+1 Query Elimination)**:
  Never execute database queries inside iterative loops:
  ```typescript
  // INCORRECT (N+1 Disaster):
  const users = await db.user.findMany();
  for (const user of users) {
    user.posts = await db.post.findMany({ where: { userId: user.id } }); // N extra queries
  }

  // CORRECT (Single Query Join / Batch Fetch):
  const usersWithPosts = await db.user.findMany({
    include: { posts: true }
  });
  ```
- **Rule 5 (Atomic Multi-Row Transactions)**:
  Any operation that updates multiple tables or balances must be wrapped in an explicit transaction (`db.transaction(...)`).

## Decision Criteria
```text
IF building high-throughput edge/serverless Next.js app:
  Choose Drizzle ORM for minimal runtime footprint.
IF column values are fixed categories (e.g. 'draft' | 'published' | 'archived'):
  Use Database Enum or string with database CHECK constraint.
IF table exceeds 100,000 rows:
  Enforce composite indexes on common filter + sort paths.
```

## Recommended Workflow
1. Draft Entity-Relationship (ER) model identifying 1-to-1, 1-to-N, and N-to-N relationships.
2. Define schema in `schema.prisma` or `schema.ts` (Drizzle).
3. Add foreign key relations and index directives.
4. Run migration generator: `npx drizzle-kit generate` or `npx prisma migrate dev`.
5. Review generated SQL migration file to verify zero accidental table drops.
6. Commit migration files to version control.

## Best Practices
- Every table must have `created_at` (`timestamp with time zone default now()`) and `updated_at` timestamps.
- Use soft deletes (`deleted_at timestamp null`) only when regulatory compliance or audit retention requires it; otherwise prefer hard deletes with cascade.
- Use connection pooling (PgBouncer or Supabase/Neon connection poolers) when connecting from serverless environments.

## Anti-Patterns
- **The Loop-Query (N+1)**: Querying the database 100 times to render 100 list items.
- **Missing Foreign Key Constraints**: Managing relationships in application code without database foreign keys, leading to orphaned records.
- **Unindexed Foreign Keys**: Joining tables on unindexed columns, degrading query speed from 2ms to 2000ms as tables grow.

## Validation Checklist
- [ ] All foreign keys are indexed.
- [ ] Primary keys use non-enumerable IDs (UUIDv7, CUID2).
- [ ] Zero queries run inside `forEach` or `map` loops.
- [ ] Schema migrations are tested and committed to Git.

## Related Skills
- `skills/backend/api-design.md`
- `skills/saas/multi-tenancy.md`
- `skills/performance/caching.md`

## References
- Drizzle ORM Documentation — https://orm.drizzle.team/
- Prisma Documentation — https://www.prisma.io/docs
- Use The Index, Luke! — Database Indexing Guide — https://use-the-index-luke.com/

## Last Reviewed
2026-09-15

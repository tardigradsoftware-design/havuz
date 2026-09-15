# Multi-Tenancy Architecture, Isolation & Row-Level Security (RLS)

## Purpose
Guarantees absolute data isolation between tenant organizations in shared multi-tenant SaaS environments, eliminating cross-tenant data leaks and unauthorized access.

## When To Use
- When engineering multi-tenant SaaS platforms, B2B data stores, and shared cloud applications.
- When configuring PostgreSQL Row-Level Security (RLS), ORM tenant middleware, or schema-per-tenant architectures.

## When Not To Use
- For dedicated single-tenant on-premise deployments or personal consumer apps.

## Core Principles
1. **Zero Cross-Tenant Leaks**: Under no circumstances may Tenant A ever observe, modify, or infer the existence of Tenant B's data records.
2. **Defense in Depth**: Enforce tenant isolation at both the application query layer AND the database engine layer (RLS).
3. **Tenant Context Propagation**: Propagate the verified `tenantId` through every async request execution context.

## Rules
- **Rule 1 (The Shared Schema Tenant Column Rule)**:
  In standard shared-database multi-tenancy:
  - Every tenant-scoped database table must include a `tenant_id` foreign key column.
  - The `tenant_id` column must be non-nullable (`NOT NULL`) and indexed.
  - Every unique constraint must be compound-scoped to the tenant:
    ```sql
    -- INCORRECT: Prevents different tenants from having a project named "Marketing"
    CREATE UNIQUE INDEX idx_projects_name ON projects (name);

    -- CORRECT: Scoped per tenant
    CREATE UNIQUE INDEX idx_projects_tenant_name ON projects (tenant_id, name);
    ```
- **Rule 2 (PostgreSQL Row-Level Security - RLS)**:
  Protect sensitive tables using database RLS policies:
  ```sql
  ALTER TABLE projects ENABLE ROW LEVEL SECURITY;

  CREATE POLICY tenant_isolation_policy ON projects
    FOR ALL
    USING (tenant_id = current_setting('app.current_tenant_id', true)::uuid);
  ```
  On every database connection/transaction checkout:
  `SET LOCAL app.current_tenant_id = 'tenant-uuid';`
- **Rule 3 (ORM Tenant Filter Middleware)**:
  If not using database-level RLS, implement mandatory ORM extension filters (e.g. Prisma client extensions or Drizzle custom operators) that automatically inject `where: { tenantId }` into every `find`, `update`, and `delete` query.
- **Rule 4 (Object Storage Isolation)**:
  Store uploaded tenant files in isolated S3 object prefixes:
  `s3://bucket/tenants/{tenantId}/uploads/{fileId}.pdf`.
  Never allow a user from Tenant A to generate presigned download URLs for files in Tenant B's prefix.

## Decision Criteria
```text
IF deploying standard SaaS with up to 100,000 tenants:
  Use Shared Database, Shared Schema with tenant_id column + PostgreSQL RLS.
IF enterprise customer pays for strict physical regulatory isolation (HIPAA, FedRAMP):
  Provision dedicated database or isolated schema per enterprise tenant.
IF query lacks tenantId filter in code review:
  FAIL CI build; all queries touching tenant tables must specify tenant boundary.
```

## Recommended Workflow
1. Add `tenant_id` to all domain models in schema definitions.
2. Create compound indexes on `(tenant_id, created_at)` and `(tenant_id, id)`.
3. Configure RLS policies or ORM tenant context wrapper.
4. Resolve active tenant from verified session in middleware.
5. Write adversarial security tests: Attempt to query Tenant B's records using Tenant A's session; assert HTTP 403/404.

## Best Practices
- Audit and log any query attempt that touches records belonging to a different tenant.
- Use automated integration test suites that spin up two distinct tenants and verify complete query segregation.
- Ensure background worker jobs always carry explicit `tenantId` in their payload metadata.

## Anti-Patterns
- **The Missing WHERE Clause**: `SELECT * FROM invoices WHERE id = :invoiceId` (Allows any attacker to enumerate and view every invoice in the company).
- **Global Unique Constraints**: Forbidding a user in Org 2 from creating a tag "Urgent" because Org 1 already created a tag named "Urgent".
- **Client-Supplied Tenant IDs**: Trusting a `tenantId` passed in from the client request body without validating that the authenticated session belongs to that tenant.

## Validation Checklist
- [ ] All tenant-owned tables have indexed `tenant_id` columns.
- [ ] Unique indexes include `tenant_id` as a prefix.
- [ ] Adversarial cross-tenant query tests pass with 0 leaks.
- [ ] File uploads are segregated by tenant prefixes.

## Related Skills
- `skills/saas/saas-architecture.md`
- `skills/backend/authorization.md`
- `skills/security/owasp.md`

## References
- AWS SaaS Architecture Fundamentals: Multi-Tenant Data Isolation
- Supabase Row Level Security Guide — https://supabase.com/docs/guides/auth/row-level-security

## Last Reviewed
2026-09-15

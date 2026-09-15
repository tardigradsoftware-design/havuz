# Authorization, RBAC & Policy-Based Access Control

## Purpose
Establishes fine-grained, secure, and verifiable permission systems (RBAC and PBAC) ensuring users only perform operations they are explicitly authorized to execute.

## When To Use
- In all multi-user, multi-tenant, SaaS, or admin-controlled applications.
- When restricting access to documents, settings, billing, or member management.

## When Not To Use
- For completely unauthenticated, public read-only content.

## Core Principles
1. **Never Rely on Client-Side Checks**: UI state hiding (`if (!isAdmin) return null`) is purely for visual UX. Every backend action must independently verify authorization.
2. **Deny by Default**: Access must be explicitly granted; if no policy explicitly allows an action, it is forbidden.
3. **Tenant Scoping as a Hard Barrier**: In multi-tenant systems, every single database query must include the tenant identifier (`tenantId` / `orgId`) to prevent cross-tenant data leaks (BOLA/IDOR).

## Rules
- **Rule 1 (The Ownership / Tenant Filter Rule)**:
  Never fetch or mutate a record solely by its primary ID:
  ```typescript
  // VULNERABLE TO IDOR (Broken Object-Level Authorization):
  await db.project.delete({ where: { id: projectId } });

  // SECURE (Scoped to User & Tenant):
  const deleted = await db.project.deleteMany({
    where: {
      id: projectId,
      tenantId: session.user.tenantId, // Mandatory boundary check
    },
  });
  if (deleted.count === 0) throw new ForbiddenError();
  ```
- **Rule 2 (Role-Based Access Control - RBAC Matrix)**:
  Define explicit permission sets instead of hardcoding role string checks:
  ```typescript
  export const ROLES = {
    OWNER: ['org:delete', 'org:update', 'members:manage', 'billing:manage', 'project:create', 'project:delete'],
    ADMIN: ['org:update', 'members:manage', 'project:create', 'project:delete'],
    MEMBER: ['project:create', 'project:view'],
    VIEWER: ['project:view'],
  } as const;

  export type Permission = (typeof ROLES)[keyof typeof ROLES][number];

  export function hasPermission(userRole: keyof typeof ROLES, required: Permission): boolean {
    return ROLES[userRole]?.includes(required) ?? false;
  }
  ```
- **Rule 3 (Centralized Policy Helper)**:
  Enforce authorization using centralized assertions:
  ```typescript
  export async function assertCan(action: Permission, tenantId: string) {
    const session = await requireAuth();
    if (session.user.tenantId !== tenantId) throw new ForbiddenError('Cross-tenant forbidden');
    if (!hasPermission(session.user.role, action)) {
      throw new ForbiddenError(`Missing permission: ${action}`);
    }
  }
  ```

## Decision Criteria
```text
IF permission is based purely on user rank (Admin vs Member):
  Use RBAC (Role-Based Access Control).
IF permission depends on resource attributes (Author can edit only their own draft):
  Use ABAC (Attribute-Based Access Control) or Policy functions.
IF request lacks verified permission:
  Return HTTP 403 Forbidden (or HTTP 404 to conceal resource existence).
```

## Recommended Workflow
1. Define roles and permissions matrix in `@/lib/permissions.ts`.
2. Attach user role and organization ID to verified session context.
3. Place `assertCan()` or tenant filter at the top of every Server Action and Route Handler.
4. Scope all database read/write queries with `where: { tenantId }`.
5. Write integration tests attempting unauthorized cross-tenant operations to verify failure.

## Best Practices
- Audit and log sensitive authorization failures to detect credential misuse or crawling attacks.
- Keep permission checks close to the database access layer to avoid missing checks in new endpoints.
- Return 404 instead of 403 when checking resource existence if revealing the existence of a private ID leaks competitive information.

## Anti-Patterns
- **IDOR / BOLA Vulnerability**: Taking `projectId` from URL params and updating it without verifying that `project.tenantId === user.tenantId`.
- **Client-Side Admin Illusion**: Checking `user.role === 'admin'` only in React component render, while leaving the Server Action wide open.
- **Role String Sprawl**: Scattering `if (user.role === 'manager' || user.role === 'boss')` throughout 40 different files.

## Validation Checklist
- [ ] Every database mutation filters by user or tenant ID.
- [ ] No endpoints rely solely on primary ID without authorization verification.
- [ ] Roles and permissions are defined in a centralized matrix.
- [ ] Unauthorized requests return 403 Forbidden or 404 Not Found.

## Related Skills
- `skills/backend/authentication.md`
- `skills/security/owasp.md`
- `skills/saas/multi-tenancy.md`

## References
- OWASP Authorization Cheat Sheet — https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html
- OWASP API Security Top 10: Broken Object Level Authorization (BOLA)

## Last Reviewed
2026-09-15

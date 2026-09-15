# SaaS Team Roles, Permissions (RBAC) & Invite Workflows

## Purpose
Establishes secure, flexible Role-Based Access Control (RBAC), team membership management, and email invite workflows for multi-user SaaS organizations.

## When To Use
- When engineering organization member lists, team settings, role assignments, and member invitation flows.
- When restricting administrative actions (billing, domain setup, member removal) to authorized roles.

## When Not To Use
- For single-user consumer applications with no team collaboration.

## Core Principles
1. **Granular Permissions Behind Roles**: Define system capabilities as explicit permissions (`project:create`, `billing:manage`), and map roles to collections of permissions. Check permissions, not role names, in business logic.
2. **Secure Tokenized Invitations**: Member invitations must use cryptographically secure, time-limited tokens that verify the recipient's identity upon acceptance.
3. **Owner Preservation**: An organization must always have at least one active Owner. Never allow the last Owner to leave or be demoted without transferring ownership.

## Rules
- **Rule 1 (Standard SaaS Role Hierarchy)**:
  Implement standard B2B roles:
  - `OWNER`: Full control, organization deletion, billing management, ownership transfer.
  - `ADMIN`: Member management, project creation/deletion, workspace settings.
  - `MEMBER`: Regular read/write access to projects and team resources.
  - `VIEWER`: Read-only access; cannot create, edit, or delete resources.
- **Rule 2 (Cryptographic Invite Flow)**:
  When inviting a new team member:
  - Generate a secure random token: `crypto.randomBytes(32).toString('hex')`.
  - Store invitation record in database: `email`, `orgId`, `role`, `tokenHash` (SHA-256 of token), `expiresAt` (7 days).
  - Send email containing link: `https://app.example.com/invites/accept?token={rawToken}`.
  - Upon acceptance: verify token, match authenticated user email with invite email (or allow user to link account), create Membership record, and delete/consume invitation.
- **Rule 3 (The Last-Owner Guard)**:
  Before processing an ownership removal or role demotion, verify that at least one other active user holds the `OWNER` role in that organization:
  ```typescript
  export async function demoteMember(orgId: string, memberId: string, newRole: Role) {
    const member = await db.membership.findUnique({ where: { id: memberId } });
    if (member.role === 'OWNER' && newRole !== 'OWNER') {
      const ownerCount = await db.membership.count({
        where: { orgId, role: 'OWNER' },
      });
      if (ownerCount <= 1) {
        throw new Error('Cannot demote the only remaining organization owner. Transfer ownership first.');
      }
    }
    return db.membership.update({ where: { id: memberId }, data: { role: newRole } });
  }
  ```
- **Rule 4 (Revocation & Expiration)**:
  Allow organization Admins to revoke pending invites at any time. Expire pending invites automatically after 7 days.

## Decision Criteria
```text
IF user role is VIEWER:
  Disable or hide "Create", "Edit", and "Delete" buttons in UI; reject any mutation attempts on backend.
IF user attempts to delete an organization:
  Require OWNER role and force password or typed confirmation ("type organization name to confirm").
IF invited user already has an active account:
  Add membership directly to their existing account upon invite token acceptance.
```

## Recommended Workflow
1. Define roles and permissions matrix in `@/lib/permissions.ts`.
2. Build team members management table in organization settings.
3. Implement invite modal with email input and role select dropdown.
4. Create secure invite acceptance endpoint and UI.
5. Guard mutation endpoints using permission assertion helpers.

## Best Practices
- Display pending invites in a distinct tab/section within team management settings.
- Allow resending invitation emails with updated expiration timestamps.
- Log all role modifications and member removals in an audit log table.

## Anti-Patterns
- **Role String Hardcoding**: Writing `if (user.role === 'admin')` throughout 30 components; if you later introduce an `editor` role, you have to rewrite the entire app.
- **Permanent Open Invitations**: Creating invite links that never expire and can be clicked 2 years later by unintended recipients.
- **The Accidental Orphaned Org**: Allowing an owner to delete their account while leaving an organization with 50 paying members with no owner.

## Validation Checklist
- [ ] Roles are mapped to granular permissions.
- [ ] Invite tokens are cryptographically secure and expire in 7 days.
- [ ] Organization must always have at least one active Owner.
- [ ] Member management endpoints enforce ADMIN/OWNER permissions.

## Related Skills
- `skills/backend/authorization.md`
- `skills/saas/saas-architecture.md`
- `skills/security/owasp.md`

## References
- OWASP Role-Based Access Control Guidelines — https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html
- Auth0 RBAC Best Practices

## Last Reviewed
2026-09-15

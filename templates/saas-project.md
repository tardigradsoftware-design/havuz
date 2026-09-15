# Template: Multi-Tenant B2B SaaS Application Blueprint

> **Archetype**: Multi-Tenant B2B Subscription Platform  
> **Key Focus**: Workspaces, RBAC permissions, Stripe subscription billing, high-density data tables, and user onboarding.

---

## 1. Executive Summary & Specification
- **Target Audience**: B2B teams, enterprise operators, and system administrators.
- **Conversion Goal**: Free-trial signups, team invites, paid plan upgrades.
- **Key Metrics**: Time-to-Value (TTV) < 2 minutes, 0 cross-tenant data leaks, 99.9% uptime.

---

## 2. Recommended File Hierarchy

```text
src/
├── app/
│   ├── (auth)/
│   │   ├── login/page.tsx        # Passwordless magic link / OAuth login
│   │   └── signup/page.tsx       # 2-step team account creation
│   ├── (dashboard)/
│   │   ├── [orgSlug]/
│   │   │   ├── layout.tsx        # Workspace sidebar, breadcrumbs, user switcher
│   │   │   ├── overview/page.tsx # KPI metric cards, recent activity feed
│   │   │   ├── projects/page.tsx # Paginated data table with search & filters
│   │   │   └── settings/
│   │   │       ├── general/page.tsx
│   │   │       ├── members/page.tsx   # Team RBAC & invite modal
│   │   │       └── billing/page.tsx   # Stripe Customer Portal link & plan tiers
│   └── api/
│       ├── webhooks/stripe/route.ts   # Signed billing events processor
│       └── auth/[...nextauth]/route.ts
├── features/
│   ├── auth/
│   ├── projects/
│   ├── billing/
│   └── organizations/
└── lib/
    ├── auth.ts                   # Auth.js / NextAuth configuration
    ├── db.ts                     # Drizzle / Prisma ORM client with RLS
    └── stripe.ts                 # Stripe SDK singleton
```

---

## 3. Mandatory Skill Set to Load

1. `skills/saas/saas-architecture.md`
2. `skills/saas/dashboard-ux.md`
3. `skills/saas/multi-tenancy.md`
4. `skills/saas/roles-permissions.md`
5. `skills/saas/onboarding.md`
6. `skills/backend/api-design.md`
7. `skills/security/owasp.md`
8. `skills/testing/playwright.md`

---

## 4. Execution Pipeline

1. **Step 1**: Define database schema (Users, Orgs, Memberships, Projects) with tenant ID keys.
2. **Step 2**: Wire authentication and session resolution in server middleware.
3. **Step 3**: Build dashboard layout with sidebar and `Cmd+K` command menu.
4. **Step 4**: Implement Stripe subscription checkout and webhook fulfillment.
5. **Step 5**: Write Playwright journey tests for workspace onboarding and invite workflows.
6. **Step 6**: Execute `checklists/security-audit.md` and `checklists/architecture-review.md`.

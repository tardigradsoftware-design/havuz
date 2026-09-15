# Modern B2B SaaS Architecture & Subscription Subsystems

## Purpose
Establishes the foundational full-stack architecture for multi-tenant B2B SaaS applications, managing organization hierarchies, subscription billing lifecycles, and team collaboration.

## When To Use
- When engineering B2B software products, enterprise portals, team workspaces, and subscription-based platforms.
- When organizing tenant boundaries, Stripe billing integrations, and organization settings.

## When Not To Use
- For single-tenant static sites or consumer e-commerce storefronts without organization workspaces.

## Core Principles
1. **Multi-Tenancy from Day Zero**: Design all data models and permission gates around organizations/workspaces rather than standalone individual users.
2. **Subscription State Synchronization**: Treat subscription status (active, past_due, canceled, trialing) as a first-class gate for feature access.
3. **Workspace Isolation**: Guarantee complete logical separation between organizational workspaces across databases, storage, and caching.

## Rules
- **Rule 1 (Organization Data Model Standard)**:
  Structure users and organizations with an explicit membership join table:
  ```text
  User (1) ───< Membership (N) >─── (1) Organization
  ```
  - `User`: Global credentials, personal profile, email.
  - `Organization`: Workspace settings, custom domain, Stripe customer ID, subscription status.
  - `Membership`: Connects User to Organization with specific `role` (`OWNER`, `ADMIN`, `MEMBER`, `VIEWER`).
- **Rule 2 (Stripe Billing Lifecycle)**:
  Support the full subscription state lifecycle:
  - `trialing`: Access granted with countdown banner ("5 days left in trial").
  - `active`: Full access granted.
  - `past_due`: Grace period banner displayed; prompt customer to update payment method before revoking access.
  - `canceled`: Restrict workspace to read-only or tier-0 free limits.
- **Rule 3 (Customer Portal Integration)**:
  Allow customers to manage their billing, view past PDF invoices, and upgrade/downgrade plans via the Stripe Customer Portal:
  ```typescript
  import { stripe } from '@/lib/stripe';

  export async function createBillingPortalSession(orgId: string) {
    const org = await db.organization.findUnique({ where: { id: orgId } });
    const session = await stripe.billingPortal.sessions.create({
      customer: org.stripeCustomerId,
      return_url: `${process.env.NEXT_PUBLIC_APP_URL}/settings/billing`,
    });
    return session.url;
  }
  ```
- **Rule 4 (Workspace Slug Routing)**:
  Use clean URL routing for multi-tenant workspaces:
  `app.example.com/[orgSlug]/projects` or `[orgSlug].example.com/projects`.

## Decision Criteria
```text
IF user belongs to multiple organizations:
  Provide workspace switcher dropdown in main sidebar; store active organization in cookie/session.
IF organization subscription is past_due:
  Display warning banner; DO NOT immediately lock database or delete tenant records.
IF organization is deleted:
  Cancel active Stripe subscription immediately via API before removing local records.
```

## Recommended Workflow
1. Scaffold User, Organization, and Membership schemas with Drizzle/Prisma.
2. Implement organization creation and member invite workflows.
3. Integrate Stripe Checkout for subscription checkout and Stripe Customer Portal for self-service billing.
4. Set up webhook listener for `customer.subscription.*` events.
5. Gate premium features behind organization subscription checks.

## Best Practices
- Cache organization subscription status in Redis or encrypted session to prevent querying Stripe or database on every single request.
- Provide a 14-day free trial with no credit card required to reduce signup friction.
- Implement soft limits with friendly upgrade prompts when organizations approach usage limits (e.g. 90/100 team members).

## Anti-Patterns
- **Binding Everything to User ID**: Attaching projects and invoices directly to `userId` instead of `orgId`, making team collaboration impossible later.
- **Querying Stripe on Page Load**: Calling the Stripe API during Server Component rendering to check subscription status (Adds 600ms latency to every page).
- **Hard Paywalls on Account Overdue**: Instantly locking enterprise users out the second a credit card renewal fails, infuriating paying customers.

## Validation Checklist
- [ ] Users can create and switch between multiple organizations.
- [ ] Stripe webhook processes subscription status updates idempotently.
- [ ] Self-serve billing portal is accessible via settings.
- [ ] Feature gates check organization subscription tier.

## Related Skills
- `skills/saas/multi-tenancy.md`
- `skills/saas/roles-permissions.md`
- `skills/backend/authorization.md`

## References
- Stripe SaaS Subscription Guide — https://stripe.com/docs/billing/subscriptions/overview
- Taxonomy SaaS Open-Source Architecture — https://github.com/shadcn-ui/taxonomy

## Last Reviewed
2026-09-15

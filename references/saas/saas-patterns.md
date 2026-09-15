# B2B SaaS Architecture & Multi-Tenancy Patterns

## 1. Tenant Data Isolation Models

| Model | Pros | Cons | Recommended For |
| :--- | :--- | :--- | :--- |
| **Shared DB, Shared Schema (tenant_id)** | Cost effective, simple migrations, highly scalable | Requires strict query discipline or RLS | 95% of standard B2B SaaS apps |
| **Shared DB, Schema-per-Tenant** | Logical separation, independent backups | Migration overhead across 1,000 schemas | Regulated B2B industries |
| **Database-per-Tenant** | Total physical isolation, custom tuning | High infrastructure cost, connection limits | High-value Enterprise contracts ($50k+/yr) |

---

## 2. Standard B2B SaaS Role Matrix

```text
Permission             OWNER   ADMIN   MEMBER   VIEWER
──────────────────────────────────────────────────────
org:delete               ✓       ✗       ✗        ✗
org:transfer_ownership   ✓       ✗       ✗        ✗
billing:manage           ✓       ✓       ✗        ✗
members:invite           ✓       ✓       ✗        ✗
members:remove           ✓       ✓       ✗        ✗
projects:create          ✓       ✓       ✓        ✗
projects:edit            ✓       ✓       ✓        ✗
projects:delete          ✓       ✓       ✗        ✗
projects:view            ✓       ✓       ✓        ✓
```

---

## 3. Stripe Subscription Webhook Event Flow

```text
Customer Subscribes ──► customer.subscription.created ──► Update DB: status = 'active'
Payment Succeeds    ──► invoice.payment_succeeded     ──► Extend subscription period
Payment Fails       ──► invoice.payment_failed        ──► Set status = 'past_due', send email
Customer Cancels    ──► customer.subscription.deleted ──► Set status = 'canceled'
```

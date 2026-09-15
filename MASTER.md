# MASTER.md — The AI Web Development Operating System Kernel

> **Central Orchestration Engine for AI Coding Agents**
> Defines the decision trees, project archetypes, dynamic skill routing, and stage-gated verification pipelines for modern web development.

---

## 1. The Core Lifecycle Engine

Every web development task executed by an AI agent must pass through the following deterministic state machine:

```text
               ┌──────────────────────────────┐
               │         INCOMING TASK        │
               └──────────────┬───────────────┘
                              │
                              ▼
               ┌──────────────────────────────┐
               │    1. IDENTIFY ARCHETYPE     │
               └──────────────┬───────────────┘
                              │
                              ▼
               ┌──────────────────────────────┐
               │ 2. DYNAMIC SKILL & REF LOAD  │
               │   (Load 4-8 Relevant Files)  │
               └──────────────┬───────────────┘
                              │
                              ▼
               ┌──────────────────────────────┐
               │  3. REQUIREMENT ENGINEERING  │
               └──────────────┬───────────────┘
                              │
                              ▼
               ┌──────────────────────────────┐
               │ 4. ARCHITECTURE & TOKENS     │
               │ (Data Flow, State, Layout)   │
               └──────────────┬───────────────┘
                              │
                              ▼
               ┌──────────────────────────────┐
               │      5. IMPLEMENTATION       │
               │ (Strict Types, Semantic DOM) │
               └──────────────┬───────────────┘
                              │
                              ▼
               ┌──────────────────────────────┐
               │     6. AUTOMATED TESTING     │
               │ (Unit, Integration, E2E)     │
               └──────────────┬───────────────┘
                              │
                              ▼
               ┌──────────────────────────────┐
               │ 7. MULTI-DISCIPLINE AUDIT    │
               │ (Visual QA, SEO, Perf, A11y, │
               │          Security)           │
               └──────────────┬───────────────┘
                              │
                              ▼
               ┌──────────────────────────────┐
               │   8. SELF-CRITIQUE & REPAIR  │
               │ (Find Flaws -> Patch Code)   │
               └──────────────┬───────────────┘
                              │
                              ▼
               ┌──────────────────────────────┐
               │    9. FINAL VERIFICATION     │
               │     (Sign-off & Release)     │
               └──────────────────────────────┘
```

---

## 2. Project Archetype Classification Matrix

Do **NOT** load the entire repository into context. Identify the project archetype from the user's brief, then selectively load only the required skill and reference files:

| Project Archetype | Primary Objectives | Mandatory Skills | Mandatory References | Primary Checklist |
| :--- | :--- | :--- | :--- | :--- |
| **Corporate / Brand Website** | High visual appeal, flawless SEO, instant load times, brand credibility. | `skills/ui/ui-design.md`<br>`skills/ui/typography.md`<br>`skills/ui/responsive-design.md`<br>`skills/seo/technical-seo.md`<br>`skills/seo/on-page-seo.md`<br>`skills/performance/core-web-vitals.md`<br>`skills/ui/accessibility.md`<br>`skills/frontend/nextjs.md` | `references/seo/google-search-essentials.md`<br>`references/ui/design-patterns.md`<br>`references/performance/web-vitals-guide.md` | `checklists/seo-audit.md`<br>`checklists/ui-review.md`<br>`checklists/performance-audit.md` |
| **SaaS Web Application** | Complex state, multi-tenancy, RBAC, high data density, zero latency. | `skills/saas/saas-architecture.md`<br>`skills/saas/dashboard-ux.md`<br>`skills/saas/roles-permissions.md`<br>`skills/frontend/react.md`<br>`skills/frontend/state-management.md`<br>`skills/backend/api-design.md`<br>`skills/security/owasp.md`<br>`skills/testing/playwright.md` | `references/saas/saas-patterns.md`<br>`references/design-systems/shadcn-radix.md`<br>`references/security/owasp-top-10.md` | `checklists/architecture-review.md`<br>`checklists/security-audit.md`<br>`checklists/testing-audit.md` |
| **E-Commerce Platform** | Frictionless checkout, conversion rate optimization, inventory state, PCI compliance. | `skills/ecommerce/ecommerce-ux.md`<br>`skills/ecommerce/product-pages.md`<br>`skills/ecommerce/checkout.md`<br>`skills/ecommerce/conversion.md`<br>`skills/seo/ecommerce-seo.md`<br>`skills/security/owasp.md`<br>`skills/performance/image-optimization.md`<br>`skills/testing/e2e.md` | `references/ecommerce/ecommerce-benchmarks.md`<br>`references/seo/google-search-essentials.md`<br>`references/performance/web-vitals-guide.md` | `checklists/performance-audit.md`<br>`checklists/security-audit.md`<br>`checklists/testing-audit.md` |
| **Analytics Dashboard** | Ultra-dense data display, tabular navigation, real-time filtering, low layout shift. | `skills/saas/dashboard-ux.md`<br>`skills/frontend/component-architecture.md`<br>`skills/ui/visual-hierarchy.md`<br>`skills/performance/javascript-performance.md`<br>`skills/ui/accessibility.md`<br>`skills/testing/visual-testing.md` | `references/ui/design-patterns.md`<br>`references/design-systems/shadcn-radix.md` | `checklists/ui-review.md`<br>`checklists/responsive-review.md` |
| **High-Conversion Landing Page** | Immediate value prop, razor-sharp typography, fast mobile paint, frictionless CTA. | `skills/ui/ui-design.md`<br>`skills/ui/visual-hierarchy.md`<br>`skills/ui/animation.md`<br>`skills/performance/core-web-vitals.md`<br>`skills/seo/on-page-seo.md`<br>`skills/ecommerce/conversion.md` | `references/ui/design-patterns.md`<br>`references/performance/web-vitals-guide.md` | `checklists/responsive-review.md`<br>`checklists/performance-audit.md` |

---

## 3. Dynamic Skill Loading Protocol

AI Agents must enforce strict token hygiene:
1. **Never load more than 8 skill files** at the initialization stage of any single task.
2. In multi-step agent interactions, drop completed verification guides and swap in implementation files only when reaching that phase.
3. If an unforeseen requirement emerges (e.g., Stripe webhook integration inside a SaaS app), pull in `skills/security/api-security.md` and `skills/backend/error-handling.md` just-in-time.

---

## 4. Phase-by-Phase Execution Guide

### Phase 1: Classification & Discovery
- Extract:
  - Target demographic & device distribution (Mobile-first vs Desktop-heavy).
  - SEO requirement level (SSR/SSG vs Private Client App).
  - Security classification (Public info vs Sensitive Financial/PII data).
- Select appropriate starter template from `/templates`.

### Phase 2: Technical Architecture & Design System
- Establish folder hierarchy (`/src/app`, `/src/components`, `/src/lib`, `/src/hooks`).
- Define the Design Tokens:
  - Spacing: 4px baseline grid (`4, 8, 12, 16, 24, 32, 48, 64px`).
  - Colors: Foreground, Background, Primary, Destructive, Muted, Border, Ring.
  - Font Stack: High-legibility sans-serif with proper `font-display: swap`.
- Component Library Strategy: Use `shadcn/ui` primitives with Radix accessibility layer; do not hand-roll unaccessible modals or dropdowns.

### Phase 3: Implementation
- Adhere to Server Components by default; opt into `'use client'` strictly for interactivity, event listeners, and browser APIs.
- Type everything strictly: No `any`, no unsafe type assertions (`as unknown as T`).
- Ensure every form has:
  - Client-side validation via Zod + React Hook Form.
  - Server-side validation via Zod.
  - Inline error feedback mapped to `aria-describedby` and `aria-invalid`.

### Phase 4: Automated Testing & Verification
- Unit test state reducers, utility formatters, and data mappers with Vitest.
- E2E test critical flows (Sign up, Log in, Search, Checkout, Filter) using Playwright.
- Run accessibility tests with `@axe-core/playwright`.

### Phase 5: Multi-Discipline Audits
Run through the checklists systematically:
1. **Visual QA**: Verify layout integrity across 375px, 768px, 1280px, and 1440px.
2. **SEO Audit**: Confirm `<title>`, `<meta description>`, OpenGraph images, canonical URLs, and valid Schema.org JSON-LD.
3. **Performance Audit**: Ensure LCP < 2.5s, CLS < 0.1, INP < 200ms. Check that large images use `next/image` with `priority` on the LCP hero image.
4. **Security Audit**: Ensure input sanitization, CSRF protection, secure cookie storage, and CSP headers.

### Phase 6: Self-Critique & Remediation
Before submitting work or declaring complete, apply the internal critique:
- *"Did I create any layout shift during dynamic content loading?"* -> Fix with fixed-height skeletons.
- *"Did I create an accessible keyboard trap or unannounced modal?"* -> Fix with `DialogPrimitive.Content` and `aria-modal="true"`.
- *"Is any unescaped string passed directly to the client?"* -> Sanitize.
- *"Did I introduce unnecessary bundle weight?"* -> Dynamic import heavy modules.

---

## 5. Conflict Resolution Rules

If two guidelines provide differing guidance, apply this resolution algorithm:
1. **Security & Accessibility Overrule Visual Aesthetics**: Never sacrifice WCAG contrast ratios or CSRF protection for stylistic effect.
2. **Web Standards Over Framework Syntaxes**: Standard HTML semantics and browser platform primitives outrank framework abstractions.
3. **Performance Budgets Over Decorative Visuals**: Never allow unoptimized decorative animations or hero videos to compromise Core Web Vitals (LCP/INP).
4. **Official Documentation Over Community Conventions**: When official Next.js or React documentation contradicts a blog post or community snippet, the official source wins.

---

*MASTER.md is the authoritative execution blueprint. All agent routines, reasoning paths, and actions must conform to this kernel.*

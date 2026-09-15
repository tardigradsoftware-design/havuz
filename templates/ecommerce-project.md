# Template: High-Performance Headless E-Commerce Blueprint

> **Archetype**: Online Retail & DTC Storefront  
> **Key Focus**: Frictionless shopping journey, instant cart mutations, high-converting PDPs, and secure Stripe checkout.

---

## 1. Executive Summary & Specification
- **Target Audience**: Online consumers browsing on mobile smartphones (70%+) and desktops.
- **Conversion Goal**: Completed checkouts, high Average Order Value (AOV), low cart abandonment.
- **Key Metrics**: Checkout abandonment < 55%, LCP < 2.0s on 4G, 0 layout shifts.

---

## 2. Recommended File Hierarchy

```text
src/
├── app/
│   ├── layout.tsx                # Announcement banner, navbar with cart badge, footer
│   ├── page.tsx                  # Hero banner, featured collections, social proof
│   ├── collections/[category]/   # Product listing page (PLP) with URL facet filters
│   ├── products/[slug]/page.tsx  # Product detail page (PDP) with variant swatches
│   ├── cart/page.tsx             # Fallback cart page (primary is slide-out drawer)
│   ├── checkout/
│   │   ├── layout.tsx            # Clean enclosed layout (no navbar/footer distractions)
│   │   └── page.tsx              # Stripe Payment Element + Address autocomplete
│   └── api/webhooks/stripe/route.ts
├── features/
│   ├── cart/                     # Cart drawer, optimistic cart hooks, line-item logic
│   └── checkout/                 # Shipping calculator, Stripe form integration
└── lib/
    ├── stripe.ts
    └── commerce.ts               # Product catalog fetchers & inventory check
```

---

## 3. Mandatory Skill Set to Load

1. `skills/ecommerce/ecommerce-ux.md`
2. `skills/ecommerce/product-pages.md`
3. `skills/ecommerce/checkout.md`
4. `skills/ecommerce/conversion.md`
5. `skills/seo/ecommerce-seo.md`
6. `skills/performance/image-optimization.md`
7. `skills/security/owasp.md`
8. `skills/testing/e2e.md`

---

## 4. Execution Pipeline

1. **Step 1**: Scaffold product schemas with variants, color swatches, and stock levels.
2. **Step 2**: Implement interactive PDP with sticky mobile buy bar and responsive gallery.
3. **Step 3**: Build slide-over cart drawer with dynamic free-shipping progress meter.
4. **Step 4**: Implement distraction-free checkout with Stripe Elements.
5. **Step 5**: Configure Product & Offer Schema JSON-LD.
6. **Step 6**: Run Playwright end-to-end checkout test simulating purchase.
7. **Step 7**: Execute `checklists/performance-audit.md` and `checklists/security-audit.md`.

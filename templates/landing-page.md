# Template: High-Conversion Lead Generation & Product Launch Blueprint

> **Archetype**: Single-Page Product Launch / Lead Funnel  
> **Key Focus**: Instant value proposition, razor-sharp typography, social proof, and frictionless conversion.

---

## 1. Executive Summary & Specification
- **Target Audience**: First-time website visitors from search, social ads, and Product Hunt.
- **Conversion Goal**: Email waitlist signup, demo booking, or direct product purchase.
- **Key Metrics**: Conversion rate > 8%, LCP < 1.8s, First Load JS < 70 kB.

---

## 2. Recommended File Hierarchy

```text
src/
├── app/
│   ├── layout.tsx                # Brand typography, minimal header, footer
│   └── page.tsx                  # Single-page high-converting narrative
├── components/
│   └── sections/
│       ├── Hero.tsx              # Bold H1, subhead, primary CTA, trust badges
│       ├── LogoCloud.tsx         # Marquee / grid of recognizable client logos
│       ├── FeatureGrid.tsx       # 3-pillar benefit breakdown with interactive tabs
│       ├── SocialProof.tsx       # Wall of love / verified customer testimonials
│       ├── PricingTable.tsx      # Annual/monthly toggle with recommended plan badge
│       ├── FAQAccordion.tsx      # Collapsible objection handlers with FAQ schema
│       └── FinalCTA.tsx          # Bottom high-contrast conversion banner
└── lib/
    └── og-image.tsx              # Dynamic Open Graph social preview generator
```

---

## 3. Mandatory Skill Set to Load

1. `skills/ui/ui-design.md`
2. `skills/ui/visual-hierarchy.md`
3. `skills/ui/animation.md`
4. `skills/seo/on-page-seo.md`
5. `skills/performance/core-web-vitals.md`
6. `skills/ecommerce/conversion.md`

---

## 4. Execution Pipeline

1. **Step 1**: Craft compelling hero value proposition with clear headline formula.
2. **Step 2**: Implement responsive layout using clean Tailwind tokens and 4px/8px grid.
3. **Step 3**: Preload hero image using `next/image` with `priority={true}`.
4. **Step 4**: Add subtle micro-interactions on CTA buttons using Motion.
5. **Step 5**: Embed Schema.org `FAQPage` and `Organization` JSON-LD.
6. **Step 6**: Execute `checklists/responsive-review.md` and `checklists/performance-audit.md`.

# Template: Corporate & Brand Website Blueprint

> **Archetype**: High-Impact Corporate / Brand Presence  
> **Key Focus**: Flawless visual hierarchy, rapid mobile loading, technical SEO, and brand storytelling.

---

## 1. Executive Summary & Specification
- **Target Audience**: Prospective enterprise clients, investors, and talent.
- **Conversion Goal**: Contact form submissions, sales meeting bookings, PDF case study downloads.
- **Key Metrics**: LCP < 2.0s, SEO 100/100, 100% WCAG AA compliance.

---

## 2. Recommended File Hierarchy

```text
src/
├── app/
│   ├── layout.tsx                # Global brand header, footer, font & metadata
│   ├── page.tsx                  # Homepage: Hero -> Proof -> Value -> Case Studies -> CTA
│   ├── about/page.tsx            # Company history, leadership, vision
│   ├── services/page.tsx         # Capabilities & service offerings
│   ├── case-studies/[slug]/      # Customer success stories with metrics
│   ├── contact/page.tsx          # Interactive inquiry form & office map
│   ├── robots.ts                 # Crawler directives
│   └── sitemap.ts                # Dynamic XML sitemap
├── components/
│   ├── ui/                       # shadcn/ui buttons, cards, dialogs
│   └── sections/                 # HeroSection, SocialProofRail, TestimonialsGrid
└── lib/
    ├── metadata.ts               # Canonical & Open Graph generators
    └── schema.ts                 # Organization & Breadcrumb JSON-LD
```

---

## 3. Mandatory Skill Set to Load

1. `skills/ui/ui-design.md`
2. `skills/ui/visual-hierarchy.md`
3. `skills/ui/responsive-design.md`
4. `skills/ui/typography.md`
5. `skills/seo/technical-seo.md`
6. `skills/seo/on-page-seo.md`
7. `skills/performance/core-web-vitals.md`
8. `skills/ui/accessibility.md`

---

## 4. Execution Pipeline

1. **Step 1**: Configure `next/font` with brand typography and Tailwind color tokens.
2. **Step 2**: Implement root layout with sticky brand navigation and accessible mobile drawer.
3. **Step 3**: Build homepage sections adhering to the Z-pattern reading flow.
4. **Step 4**: Embed Schema.org `Organization` JSON-LD in `layout.tsx`.
5. **Step 5**: Execute `checklists/seo-audit.md` and `checklists/ui-review.md`.

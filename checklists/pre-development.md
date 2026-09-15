# Pre-Development & Discovery Checklist

> **Stage**: Project Kickoff & Scoping  
> **Mandatory**: Must be reviewed before writing application code.

---

## 1. Project Archetype & Objectives
- [ ] Project archetype identified (Corporate Web, SaaS, E-Commerce, Dashboard, Landing Page).
- [ ] Target audience and primary conversion goals documented.
- [ ] Performance and accessibility targets established (e.g., WCAG 2.2 AA, LCP < 2.5s).

## 2. Technology Stack & Toolchain
- [ ] TypeScript strict mode enabled in `tsconfig.json`.
- [ ] Linter (ESLint/Biome) and formatter (Prettier) configured.
- [ ] Design system primitives configured (`shadcn/ui`, Tailwind CSS v4, Lucide).
- [ ] Package manager locked (`pnpm-lock.yaml`, `package-lock.json`, or `bun.lockb`).

## 3. Data & Boundary Planning
- [ ] Primary entity models and relationships drafted.
- [ ] Server Component vs Client Component boundaries mapped.
- [ ] External third-party integrations (Stripe, Auth, Resend) identified.

---
*Sign-off: Architect / Lead Agent*

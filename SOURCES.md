# SOURCES.md — Curated Source Registry & Rejection Log

> **Audit Date**: 2026-09-15  
> **Standard**: Every referenced framework, library, specification, and repository has been vetted across GitHub stars, release activity, real-world industry adoption, official backing, license permissiveness, and AI coding agent utility.

---

## 1. Evaluation Taxonomy

- **TIER 1 (CORE)**: Canonical, actively maintained, battle-tested foundations. Non-negotiable for modern production applications.
- **TIER 2 (REFERENCE)**: High-quality patterns, complementary libraries, and domain references to be imported contextually.
- **TIER 3 (OPTIONAL)**: Useful for specialized architectures, edge cases, or specific developer niches.
- **REJECTED**: Deprecated, bloated, unmaintained, anti-pattern, or incompatible with modern Server Components / React 19 / Agentic workflows.

---

## 2. Tier 1 — Core Sources

| Source | Category | Tier | Official? | License | Why Selected | URL | Last Checked |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **React** (`react/react`) | Frontend Framework | Tier 1 | Yes (Meta) | MIT | The undisputed industry baseline for UI component engineering; native React 19 Actions and RSC. | https://github.com/facebook/react | 2026-09-15 |
| **Next.js** (`vercel/next.js`) | Full-Stack Meta-Framework | Tier 1 | Yes (Vercel) | MIT | Industry-standard production framework with Server Components, App Router, asset optimization, and edge routing. | https://github.com/vercel/next.js | 2026-09-15 |
| **TypeScript** (`microsoft/TypeScript`) | Language Standard | Tier 1 | Yes (Microsoft) | Apache-2.0 | Mandatory type safety, structural typing, and interface contracts required to eliminate runtime errors. | https://github.com/microsoft/TypeScript | 2026-09-15 |
| **Tailwind CSS** (`tailwindlabs/tailwindcss`) | Utility CSS Framework | Tier 1 | Yes (Tailwind Labs) | MIT | De-facto utility-first styling engine; zero runtime CSS injection; compile-time CSS variable extraction. | https://github.com/tailwindlabs/tailwindcss | 2026-09-15 |
| **shadcn/ui** (`shadcn-ui/ui`) | Component System | Tier 1 | Yes | MIT | Copy-into-codebase composable architecture built on Radix primitives; perfect for AI code generation. | https://github.com/shadcn-ui/ui | 2026-09-15 |
| **Radix Primitives** (`radix-ui/primitives`) | Headless Accessibility | Tier 1 | Yes (WorkOS) | MIT | Unstyled, fully accessible UI primitives with comprehensive WAI-ARIA and keyboard focus management. | https://github.com/radix-ui/primitives | 2026-09-15 |
| **Playwright** (`microsoft/playwright`) | E2E & Browser Automation | Tier 1 | Yes (Microsoft) | Apache-2.0 | Reliable cross-browser testing, automated waiting, trace viewing, and headless visual QA validation. | https://github.com/microsoft/playwright | 2026-09-15 |
| **OWASP Cheat Sheet Series** (`OWASP/CheatSheetSeries`) | Web Security Standard | Tier 1 | Yes (OWASP) | CC-BY-SA-4.0 | Authoritative, actionable guidance for authentication, authorization, session management, CSRF, and XSS. | https://github.com/OWASP/CheatSheetSeries | 2026-09-15 |
| **Google Web Vitals** (`GoogleChrome/web-vitals`) | Performance Measurement | Tier 1 | Yes (Google Chrome) | Apache-2.0 | Canonical library for measuring real-user Core Web Vitals (LCP, CLS, INP) in production environments. | https://github.com/GoogleChrome/web-vitals | 2026-09-15 |
| **Zod** (`colinhacks/zod`) | Schema Validation | Tier 1 | Yes | MIT | Declarative TypeScript-first schema validation for API boundaries, forms, and environment variables. | https://github.com/colinhacks/zod | 2026-09-15 |

---

## 3. Tier 2 — Reference Sources

| Source | Category | Tier | Official? | License | Why Selected | URL | Last Checked |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TanStack Query** (`TanStack/query`) | Server State Management | Tier 2 | Yes (TanStack) | MIT | Declarative async state manager for client-side fetching, caching, deduplication, and optimistic updates. | https://github.com/TanStack/query | 2026-09-15 |
| **Motion** (`motiondivision/motion`) | Animation Engine | Tier 2 | Yes (Motion Div) | MIT | Industry standard for declarative React gesture physics, layout animations, and exit transitions. | https://github.com/motiondivision/motion | 2026-09-15 |
| **Lucide Icons** (`lucide-icons/lucide`) | Iconography System | Tier 2 | Yes | ISC | Clean, consistent, tree-shakeable SVG icon collection standard in modern design systems. | https://github.com/lucide-icons/lucide | 2026-09-15 |
| **axe-core** (`dequelabs/axe-core`) | Accessibility Engine | Tier 2 | Yes (Deque Labs) | MPL-2.0 | The automated accessibility testing engine powering Lighthouse, Chrome DevTools, and Playwright a11y. | https://github.com/dequelabs/axe-core | 2026-09-15 |
| **Vitest** (`vitest-dev/vitest`) | Unit & Integration Testing | Tier 2 | Yes (Vitest Dev) | MIT | Blazing-fast Vite-native test runner with Jest-compatible API and first-class ESM and TS support. | https://github.com/vitest-dev/vitest | 2026-09-15 |
| **React Hook Form** (`react-hook-form/react-hook-form`) | Form State Engine | Tier 2 | Yes | MIT | High-performance, un-controlled form state management with minimal re-renders and direct Zod integration. | https://github.com/react-hook-form/react-hook-form | 2026-09-15 |
| **Node Best Practices** (`goldbergyoni/nodebestpractices`) | Backend Architecture | Tier 2 | Yes | CC-BY-SA-4.0 | Comprehensive collection of Node.js production engineering patterns, error domains, and security. | https://github.com/goldbergyoni/nodebestpractices | 2026-09-15 |
| **Taxonomy** (`shadcn-ui/taxonomy`) | SaaS Reference Architecture | Tier 2 | Yes | MIT | Canonical open-source SaaS architecture showing Next.js App Router, Prisma/Drizzle, Stripe, and Auth. | https://github.com/shadcn-ui/taxonomy | 2026-09-15 |
| **Vercel Commerce** (`vercel/commerce`) | E-Commerce Reference | Tier 2 | Yes (Vercel) | MIT | High-performance Shopify/BigCommerce/custom headless e-commerce reference with instant cart mutations. | https://github.com/vercel/commerce | 2026-09-15 |
| **Drizzle ORM** (`drizzle-team/drizzle-orm`) | Database Access | Tier 2 | Yes | Apache-2.0 | Lightweight, type-safe SQL-like ORM with zero code-generation overhead and maximum serverless performance. | https://github.com/drizzle-team/drizzle-orm | 2026-09-15 |

---

## 4. Tier 3 — Optional Sources

| Source | Category | Tier | Official? | License | Why Selected | URL | Last Checked |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **CMDK** (`pacocoursey/cmdk`) | Command Palette Primitive | Tier 3 | Community | MIT | Fast, unstyled command menu component for keyboard-centric power user interfaces. | https://github.com/pacocoursey/cmdk | 2026-09-15 |
| **Vaul** (`emilkowalski/vaul`) | Mobile Drawer Primitive | Tier 3 | Community | MIT | iOS-style smooth physics-based drawer component for mobile-first web applications. | https://github.com/emilkowalski/vaul | 2026-09-15 |
| **Cal.com** (`calcom/cal.diy`) | Complex Scheduling UI | Tier 3 | Yes (Cal.com) | AGPLv3 (Inspect only) | Production reference for complex calendar grids, timezone handling, and nested booking workflows. | https://github.com/calcom/cal.com | 2026-09-15 |
| **Create T3 App** (`t3-oss/create-t3-app`) | Full-Stack Type Safety | Tier 3 | Community | MIT | Demonstrates end-to-end type safety connecting Prisma/Drizzle, tRPC/Next.js, and Tailwind CSS. | https://github.com/t3-oss/create-t3-app | 2026-09-15 |
| **Precedent** (`steven-tey/precedent`) | Modern Web Starter | Tier 3 | Community | MIT | Clean example of micro-animations, tooltips, framer-motion transitions, and Radix hooks. | https://github.com/steven-tey/precedent | 2026-09-15 |

---

## 5. Rejected Sources & Rationale

| Source / Technology | Category | Status | Detailed Reason for Rejection |
| :--- | :--- | :--- | :--- |
| **Create React App (CRA)** | Project Bootstrapper | **REJECTED** | Deprecated by React core team. Lack of SSR, lack of RSC support, outdated Webpack configurations, excessive vulnerabilities. |
| **Enzyme** | Testing Utility | **REJECTED** | Incompatible with modern React (18/19). Encourages testing component implementation details instead of user-facing DOM behavior. Superseded by Playwright and React Testing Library. |
| **Moment.js** | Date/Time Library | **REJECTED** | Officially in maintenance-only mode. Mutable API, zero tree-shaking, adds 70kB+ of bloated bundle weight. Prefer native `Intl` API or `date-fns`. |
| **Styled-Components / Emotion (CSS-in-JS)** | Styling Solutions | **REJECTED** | Runtime CSS-in-JS creates heavy hydration overhead, injects dynamic `<style>` tags that break React Server Components (RSC) streaming, and increases runtime JS execution time. Superseded by Tailwind CSS and Zero-Runtime CSS. |
| **Redux (Legacy Vanilla)** | State Management | **REJECTED** | Excessive boilerplate, complex action-dispatch-reducer ceremony. For server data, Server Components and TanStack Query eliminate 90% of state needs. For client UI state, Zustand provides 10x smaller footprint with zero friction. |
| **Axios** | HTTP Client | **REJECTED** | In modern web runtimes (Node 18+, Next.js, Edge), native `fetch` provides built-in request caching, deduplication, streaming, and AbortController without adding extra bundle payload. |
| **Bootstrap / jQuery** | UI Framework | **REJECTED** | Imperative DOM manipulation contradicts declarative React rendering. Heavy bundle overhead, lack of modern accessibility primitives compared to Radix UI. |
| **Lighthouse CI Standalone for Agents** | Auditing Tool | **REJECTED** | Flaky synthetic score fluctuations in sandboxed headless container environments. Playwright + Chrome Trace and Web Vitals telemetry offer deterministic, actionable measurements. |

---

*This registry is audited to ensure zero outdated libraries or security risks compromise the knowledge base.*

# Modern Web Frontend Blueprint (2026 Canonical Stack)

## Core Stack Architecture

```text
Language:      TypeScript 5.x (Strict mode enabled)
Runtime:       Node.js 22 LTS / Edge Runtimes
Framework:     Next.js 15+ (App Router, Server Actions, React Server Components)
View Engine:   React 19 (Actions, useActionState, useOptimistic, Suspense)
Styling:       Tailwind CSS v4 (Zero-runtime utility CSS, CSS Variables)
Components:    shadcn/ui (Radix UI headless primitives + CVA variants)
Icons:         Lucide React (Tree-shakeable SVG icons)
Validation:    Zod 3.x (Type-safe runtime schemas)
Forms:         React Hook Form + @hookform/resolvers/zod
Data Sync:     TanStack Query v5 / Server Components
Testing:       Playwright (E2E) + Vitest (Unit)
```

## Package Registry Recommendation

```json
{
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "next": "^15.0.0",
    "tailwindcss": "^4.0.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.0",
    "tailwind-merge": "^2.5.0",
    "lucide-react": "^0.450.0",
    "@radix-ui/react-dialog": "^1.1.0",
    "@radix-ui/react-dropdown-menu": "^2.1.0",
    "@radix-ui/react-slot": "^1.1.0",
    "zod": "^3.23.0",
    "nuqs": "^2.0.0"
  },
  "devDependencies": {
    "typescript": "^5.6.0",
    "@types/react": "^19.0.0",
    "@types/node": "^22.0.0",
    "vitest": "^2.1.0",
    "@playwright/test": "^1.48.0",
    "@axe-core/playwright": "^4.10.0"
  }
}
```

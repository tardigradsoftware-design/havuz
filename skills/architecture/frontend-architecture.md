# Frontend Architecture, Modular Organization & Boundaries

## Purpose
Establishes a robust, scalable directory structure and clear boundary contracts that allow complex frontend codebases to grow effortlessly across teams and features without turning into spaghetti code.

## When To Use
- When initiating a new frontend application or major feature domain.
- When refactoring sprawling, disorganized components or separating business logic from UI rendering.

## When Not To Use
- For tiny single-file demos or quick prototype scripts.

## Core Principles
1. **Feature-First Colocation**: Group files by feature/domain rather than by technical file type (e.g., place components, hooks, types, and tests together inside `features/billing/`).
2. **Explicit Public Interfaces**: Features must expose a single clean entrypoint (`index.ts`); internal helper components must remain private to that feature directory.
3. **Unidirectional Dependency Graph**: Shared primitives cannot depend on feature-specific modules. Dependencies must flow downward: Features -> Domain Components -> UI Primitives -> Utilities.

## Rules
- **Rule 1 (Canonical Directory Architecture)**:
  Organize Next.js/React applications with clean separation:
  ```text
  src/
  ├── app/                  # Route handlers, layouts, page routing shells
  │   ├── (auth)/           # Route group for authentication
  │   ├── (dashboard)/      # Route group for protected SaaS views
  │   └── layout.tsx        # Root HTML wrapper
  ├── components/           # Cross-cutting UI primitives
  │   ├── ui/               # Atomic design system primitives (button, dialog, card)
  │   └── shared/           # Generic shared widgets (navbar, footer, theme-toggle)
  ├── features/             # Business domain feature modules
  │   ├── auth/             # Login, signup, reset password logic
  │   ├── projects/         # Project list, creation, settings
  │   └── billing/          # Stripe checkout, pricing tables, invoices
  │       ├── components/   # Billing-specific UI components
  │       ├── hooks/        # Billing-specific React hooks
  │       ├── actions/      # Billing Server Actions
  │       ├── types.ts      # Billing schemas & types
  │       └── index.ts      # Public API for the billing feature
  ├── lib/                  # Infrastructure singletons (db, auth, stripe, mailer)
  └── types/                # Global type definitions
  ```
- **Rule 2 (The Public Interface Barrier)**:
  Other features may only import from `features/billing` through its `index.ts`. Never deep-import private internals like `import { calculateTax } from '@/features/billing/components/internal-tax-widget'`.
- **Rule 3 (Strict Decoupling of UI from Data Layer)**:
  Presentational components must receive plain typed data and callbacks; never embed raw database or network queries directly inside pure presentational cards.

## Decision Criteria
```text
IF a component is reused across 2 or more distinct feature domains:
  Promote component to `src/components/ui/` or `src/components/shared/`.
IF a component is specific to one business domain (e.g., ProjectCard):
  Keep colocated inside `src/features/projects/components/`.
IF a utility function is purely mathematical or generic:
  Place in `src/lib/utils.ts`.
```

## Recommended Workflow
1. Identify major business domains (Auth, Projects, Billing, Analytics).
2. Scaffold feature directories with dedicated components, actions, and types.
3. Establish atomic design system primitives in `components/ui/`.
4. Assemble application views in `app/` using feature modules.
5. Enforce boundaries using ESLint `no-restricted-imports` rules.

## Best Practices
- Keep page files (`page.tsx`) lean—they should primarily authenticate, fetch initial server data, and render the feature view.
- Maintain consistent file naming conventions (kebab-case for directories and filenames).
- Colocate unit test files directly next to the module they test (`billing.test.ts`).

## Anti-Patterns
- **The Global Dumping Ground**: Placing 250 disparate components inside a single flat `src/components/` folder.
- **Circular Feature Dependencies**: Feature A importing from Feature B while Feature B imports from Feature A.
- **Deep Nesting Hell**: Creating folder structures 8 levels deep (`src/features/a/b/c/d/e/f/g/component.tsx`).

## Validation Checklist
- [ ] Code is organized into feature-first domains.
- [ ] Features export clean public APIs via `index.ts`.
- [ ] Pages in `app/` act as lightweight orchestration shells.
- [ ] No circular dependencies exist between feature modules.

## Related Skills
- `skills/frontend/component-architecture.md`
- `skills/architecture/maintainability.md`
- `skills/frontend/nextjs.md`

## References
- Feature-Sliced Design Methodology — https://feature-sliced.design/
- Bulletproof React Architecture Blueprint — https://github.com/alan2207/bulletproof-react

## Last Reviewed
2026-09-15

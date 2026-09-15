# Component Architecture & Composition Patterns

## Purpose
Defines architectural patterns for organizing, composing, and decoupling frontend components to maximize reusability, testability, and long-term maintainability.

## When To Use
- When structuring new frontend codebases or feature directories.
- When designing UI component libraries and design system primitives.
- When refactoring large monolithic components into clean composite systems.

## When Not To Use
- For single-use, trivial inline micro-fragments where splitting creates needless indirection.

## Core Principles
1. **Composition Over Configuration**: Prefer composing smaller, focused components together over building massive "Swiss Army Knife" components with 50 configuration props.
2. **Single Responsibility Principle**: A component should either handle layout/presentation OR data fetching/state—rarely both simultaneously.
3. **Open for Extension, Closed for Modification**: Design components to accept child elements or slots (`children`, `asChild`) so consumers can customize content without editing the primitive.

## Rules
- **Rule 1 (The 200-Line Guideline)**:
  Components exceeding **200 lines of code** must be evaluated for decomposition. Extract sub-views, complex reducers, or custom hooks into adjacent modular files.
- **Rule 2 (The Slot Pattern / Polymorphic Rendering)**:
  Use Radix `Slot` (`asChild` pattern) to allow components to change their underlying HTML tag while preserving styles and behavior:
  ```tsx
  import { Slot } from '@radix-ui/react-slot';

  interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
    asChild?: boolean;
  }

  export function Button({ asChild = false, className, ...props }: ButtonProps) {
    const Comp = asChild ? Slot : 'button';
    return <Comp className={cn(buttonVariants(), className)} {...props} />;
  }
  ```
  This allows `<Button asChild><Link href="/dashboard">Go to Dashboard</Link></Button>` without invalid nested `<button><a>` DOM violations.
- **Rule 3 (Compound Component Pattern)**:
  For multi-part components (Dropdown, Modal, Card, Accordion), export compound subcomponents sharing context:
  ```tsx
  <Card>
    <CardHeader>
      <CardTitle>Project Overview</CardTitle>
      <CardDescription>Metrics for Q3</CardDescription>
    </CardHeader>
    <CardContent>...</CardContent>
    <CardFooter>...</CardFooter>
  </Card>
  ```
- **Rule 4 (Separation of Server Container & Client View)**:
  Colocate asynchronous server data loaders with presentational client components:
  - `UserProjectsContainer.tsx` (Server Component: fetches DB records).
  - `UserProjectsList.tsx` (Client Component: interactive search, filter, and sorting).

## Decision Criteria
```text
IF component has multiple sub-parts with independent rendering:
  Use Compound Component pattern (Card, CardHeader, CardContent).
IF component needs to render as a link or custom tag:
  Use Radix `Slot` (asChild pattern).
IF state logic exceeds 3 useState hooks:
  Extract into a dedicated custom hook (`useProjectFilters`).
```

## Recommended Workflow
1. Identify the core user capability and separate presentation from state.
2. Build headless or presentational primitives first with strict TypeScript prop contracts.
3. Wrap interactive behaviors into compound components or custom hooks.
4. Export components from clean barrel files or modular index paths.
5. Write isolated unit tests for the component's state transitions.

## Best Practices
- Keep props flat and intuitive (`title`, `description`, `icon`) rather than passing deep nested data structures (`data.response.user.profile.meta`).
- Use controlled/uncontrolled hybrid patterns (`value` and `defaultValue`) for form inputs.
- Colocate component styles, types, and tests in the same directory:
  ```text
  components/
    project-card/
      project-card.tsx
      project-card.test.tsx
      project-card.types.ts
  ```

## Anti-Patterns
- **The Mega-Component**: A single 1200-line `Dashboard.tsx` handling auth, data fetching, charts, modals, and CSV export.
- **Boolean Prop Explosion**: Adding `hasHeader`, `isCompact`, `showAvatar`, `withShadow`, `isLarge` instead of composing separate primitives.
- **Prop Drilling Through 5 Levels**: Passing `onDelete` down through 5 intermediate components that do not need to know about deletion.

## Validation Checklist
- [ ] No single component exceeds 200 lines without explicit architectural justification.
- [ ] Multi-part components use Compound Component or Slot patterns.
- [ ] Presentational components are decoupled from direct API fetching.
- [ ] Polymorphic elements leverage `asChild` to avoid invalid nested interactive tags.

## Related Skills
- `skills/frontend/react.md`
- `skills/frontend/tailwind.md`
- `skills/architecture/frontend-architecture.md`

## References
- React Design Patterns & Principles — Michael Chan
- Radix UI Primitives Composition Architecture — https://www.radix-ui.com/primitives/docs/overview/composition

## Last Reviewed
2026-09-15

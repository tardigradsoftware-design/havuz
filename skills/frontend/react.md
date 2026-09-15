# Modern React Engineering (React 19 Standards)

## Purpose
Establishes the definitive engineering standards for building robust, concurrent, and high-performance React applications leveraging React 19 primitives, React Server Components (RSC), and native Actions.

## When To Use
- In all React and meta-framework applications (Next.js, Vite, Astro with React).
- When architecting component lifecycles, hooks, form mutations, and async UI rendering.

## When Not To Use
- When working within pure HTML/vanilla JavaScript micro-scripts where React runtime is unnecessary.

## Core Principles
1. **Server First, Client Second**: Components render on the server by default. Only push components to the client bundle when interactivity (event handlers, local state, browser APIs) strictly requires it.
2. **Declarative Mutations**: Use React 19 Actions (`useActionState`, `useFormStatus`, `useOptimistic`) instead of manual `useState` booleans for mutation lifecycles.
3. **Data Flow Down, Events Up**: Maintain unidirectional data flow; avoid bidirectional two-way binding hacks or imperatively mutating props.

## Rules
- **Rule 1 (React 19 Action Form Pattern)**:
  Handle form mutations declaratively using `useActionState`:
  ```tsx
  import { useActionState } from 'react';
  import { updateProfile } from '@/app/actions';

  export function ProfileForm({ user }: { user: User }) {
    const [state, formAction, isPending] = useActionState(updateProfile, null);

    return (
      <form action={formAction} className="space-y-4">
        <input name="name" defaultValue={user.name} required />
        <button type="submit" disabled={isPending}>
          {isPending ? 'Saving...' : 'Save Profile'}
        </button>
        {state?.error && <p role="alert" className="text-destructive">{state.error}</p>}
      </form>
    );
  }
  ```
- **Rule 2 (Optimistic UI Updates)**:
  When modifying lists or like counts, use `useOptimistic` to provide zero-latency UI updates while network mutations resolve.
- **Rule 3 (Hook Dependency Strictness)**:
  Never disable ESLint `react-hooks/exhaustive-deps`. All reactive values (props, state, derived variables) referenced inside `useEffect`, `useCallback`, or `useMemo` must be declared in the dependency array.
- **Rule 4 (No useEffect for Data Fetching)**:
  Never write `useEffect(() => { fetch(...) }, [])` in modern React. Use React Server Components (direct async/await) or TanStack Query.
- **Rule 5 (Key Integrity)**:
  Never use array index (`key={index}`) as a React key for mutable or sortable lists. Always use stable, unique IDs (`key={item.id}`).

## Decision Criteria
```text
IF component requires browser events (onClick, onChange) or hooks (useState, useEffect):
  Mark file with 'use client' at the top.
ELSE:
  Keep as default Server Component (fetch data directly with async/await).
IF state is purely derived from props:
  Calculate during render (const fullName = `${first} ${last}`); DO NOT mirror in useState.
```

## Recommended Workflow
1. Architect data hierarchy starting from root Server Component.
2. Fetch asynchronous data directly in Server Components using `await db.query()` or `await fetch()`.
3. Wrap slow asynchronous subtrees with `<Suspense fallback={<Skeleton />}>`.
4. Isolate client interactive elements into leaf client components.
5. Implement form actions with Zod validation on the server action.

## Best Practices
- Keep client component boundaries as small as possible at the leaves of the component tree.
- Use `useTransition` for non-urgent state updates to keep input responsiveness high.
- Clean up subscriptions and timers inside `useEffect` return functions.

## Anti-Patterns
- **Prop Drilling Hell**: Passing props through 7 layers of components; resolve via component composition or Context.
- **Client Root Pollution**: Placing `'use client'` at the root layout file, destroying all Server Component streaming benefits.
- **Sync External State in useEffect**: Synchronizing two local states using `useEffect`, causing unnecessary secondary re-renders.

## Validation Checklist
- [ ] Are Server Components kept as default without unnecessary `'use client'`?
- [ ] Are all mutation forms leveraging React 19 Actions / `useActionState`?
- [ ] Are all list keys stable, unique IDs?
- [ ] Are all `useEffect` dependency arrays exhaustive with zero linter suppressions?

## Related Skills
- `skills/frontend/nextjs.md`
- `skills/frontend/state-management.md`
- `skills/frontend/component-architecture.md`

## References
- Official React 19 Documentation — https://react.dev/
- React Server Components Working Group — https://github.com/reactjs/rfcs

## Last Reviewed
2026-09-15

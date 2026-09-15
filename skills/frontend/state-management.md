# Modern State Management & Data Synchronization

## Purpose
Establishes a streamlined, modern state hierarchy that categorizes application state into URL, Server, Local UI, and Global Client state, eliminating boilerplate and synchronization bugs.

## When To Use
- When designing data flow across interactive web applications, dashboards, search pages, and SaaS tools.
- When determining whether to store data in the URL, React Server Components, TanStack Query, or Zustand.

## When Not To Use
- For completely static marketing pages where all content is fixed at build time.

## Core Principles
1. **URL as the Primary Source of Truth**: Any state that should survive page refresh or be shareable via link (search queries, filters, active tabs, page pagination, modal state) belongs in the URL query parameters.
2. **Server State is Not Client State**: Server data is asynchronous and snapshot-based. Do not duplicate server data into client stores (Redux/Zustand); manage it with Server Components or TanStack Query.
3. **Colocate UI State**: Keep ephemeral UI state (e.g., whether a dropdown is open, current text input value) inside the local component that owns it.

## Rules
- **Rule 1 (The State Hierarchy Decision Tree)**:
  1. *URL State*: Search filters, sort orders, active tabs, pagination, date ranges (`useSearchParams` or `nuqs`).
  2. *Server State*: Database records, user profiles, API responses (React Server Components or TanStack Query).
  3. *Local UI State*: Form inputs, dropdown toggles, modal open state (`useState`, `useReducer`).
  4. *Global Client State*: Theme mode, user preferences, multi-step unsaved client workflows (Zustand).
- **Rule 2 (No Legacy Redux Boilerplate)**:
  Never install or configure legacy Redux, Redux-Saga, or Action-Reducer ceremony for modern web applications. If lightweight global client state is truly required, use **Zustand**:
  ```typescript
  import { create } from 'zustand';

  interface SidebarStore {
    isOpen: boolean;
    toggle: () => void;
  }

  export const useSidebarStore = create<SidebarStore>((set) => ({
    isOpen: true,
    toggle: () => set((state) => ({ isOpen: !state.isOpen })),
  }));
  ```
- **Rule 3 (URL Query Synchronization with nuqs)**:
  When managing complex interactive filters, use type-safe URL state:
  ```typescript
  import { useQueryState, parseAsString, parseAsInteger } from 'nuqs';

  export function ProductFilters() {
    const [search, setSearch] = useQueryState('q', parseAsString.withDefault(''));
    const [page, setPage] = useQueryState('page', parseAsInteger.withDefault(1));
    // Updating `search` or `page` immediately updates browser URL without full reload.
  }
  ```
- **Rule 4 (No Storing Derived State)**:
  Never store computed values in state. Calculate derived values during render or wrap in `useMemo` if computationally expensive:
  ```typescript
  // CORRECT:
  const filteredItems = useMemo(() => items.filter((i) => i.active), [items]);
  // INCORRECT:
  // const [filteredItems, setFilteredItems] = useState([]);
  // useEffect(() => { setFilteredItems(items.filter(i => i.active)) }, [items]);
  ```

## Decision Criteria
```text
IF state should be bookmarkable or shared via link:
  Store in URL Query Parameters.
IF state comes from database / backend:
  Fetch via React Server Component or TanStack Query.
IF state is needed by only 1 component:
  Use useState / useReducer.
IF client-only state is shared across distant component trees (e.g. audio player, global drawer):
  Use Zustand.
```

## Recommended Workflow
1. Identify all stateful values required for the feature.
2. Map each value against the State Hierarchy Decision Tree.
3. Bind URL-bound states to search parameters.
4. Keep server data in Server Components or TanStack Query cache.
5. Create local `useState` only for leaf interaction state.

## Best Practices
- Debounce rapid URL query state updates (e.g., live text search inputs) by 300ms to avoid flooding browser history.
- Use optimistic updates for instant UI reaction during mutations.
- Prefer `useReducer` over multiple interdependent `useState` variables when transitions are complex.

## Anti-Patterns
- **The Redux Black Hole**: Putting every button click, text input, and API response into a monolithic global store.
- **URL Desynchronization**: Having an input filter update local state without reflecting the filter in the browser URL, making sharing impossible.
- **Duplicated Caches**: Fetching data with TanStack Query and immediately copying it into a Zustand store.

## Validation Checklist
- [ ] Are search, filter, and pagination states preserved in URL query parameters?
- [ ] Is server data fetched via Server Components or TanStack Query?
- [ ] Are computed/derived values calculated on-the-fly rather than synced via `useEffect`?
- [ ] Is global client store restricted to true global UI concerns (e.g. sidebar toggle)?

## Related Skills
- `skills/frontend/react.md`
- `skills/frontend/nextjs.md`
- `skills/saas/dashboard-ux.md`

## References
- TkDodo’s Practical React Query Guide — https://tkdodo.eu/blog/practical-react-query
- nuqs — Type-Safe Search Params State Manager for React — https://nuqs.47ng.com/

## Last Reviewed
2026-09-15

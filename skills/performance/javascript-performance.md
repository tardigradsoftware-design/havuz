# JavaScript Performance, Bundle Splitting & Hydration Optimization

## Purpose
Minimizes JavaScript bundle execution costs, eliminates client-side hydration bottlenecks, and prevents main-thread blocking to guarantee rapid Time to Interactive (TTI) and low INP.

## When To Use
- When auditing frontend bundle sizes, third-party libraries, and client component weight.
- When configuring dynamic code-splitting and optimizing heavy data processing logic.

## When Not To Use
- For server-only backend microservices where browser bundle size is irrelevant.

## Core Principles
1. **The Best JavaScript is No JavaScript**: The fastest code to parse, compile, and execute is code that never reaches the browser. Leverage React Server Components to keep dependencies on the server.
2. **Code Splitting by Route & Interaction**: Never load heavy dependencies (charts, rich-text editors, PDF generators) in the initial page bundle. Load them dynamically when required.
3. **Hydration Minimization**: Only hydrate interactive UI islands; static layout elements should remain pure unhydrated HTML.

## Rules
- **Rule 1 (Dynamic Import for Heavy Components)**:
  Any component that loads a heavy third-party library (> 30kB gzipped, such as Recharts, Monaco Editor, Lucide icon packs) must be loaded dynamically:
  ```tsx
  import dynamic from 'next/dynamic';

  const AnalyticsChart = dynamic(() => import('@/components/analytics-chart'), {
    loading: () => <div className="h-[350px] w-full animate-pulse bg-muted rounded-xl" />,
    ssr: false, // Omit SSR if canvas/browser-specific
  });
  ```
- **Rule 2 (Barrel File Elimination)**:
  Never import from large index barrel files that defeat tree-shaking:
  ```typescript
  // INCORRECT (May import entire library into bundle):
  import { Check } from 'lucide-react';
  // CORRECT (Tree-shakeable direct or optimized import):
  import Check from 'lucide-react/dist/esm/icons/check';
  ```
- **Rule 3 (Web Worker for Heavy Calculations)**:
  Offload intensive tasks (large CSV parsing, client image cropping, cryptography) from the main thread into a Web Worker using `Comlink` or native `new Worker()`.
- **Rule 4 (Bundle Size Audit Gate)**:
  Run `@next/bundle-analyzer` in CI. Any pull request that increases the first-load JS shared bundle by more than **15 kB** must be flagged for architectural review.

## Decision Criteria
```text
IF library is only needed on user interaction (e.g. clicking "Export PDF"):
  Import dynamically inside click handler: const { jsPDF } = await import('jspdf');
IF component has zero interactivity or event listeners:
  Keep as Server Component; DO NOT add 'use client'.
IF computational function takes > 16ms:
  Wrap in Web Worker or split using scheduler.yield() to prevent dropping 60fps frames.
```

## Recommended Workflow
1. Analyze bundle distribution: `ANALYZE=true npm run build`.
2. Identify top 5 largest packages in client chunks.
3. Replace heavy packages with lightweight modern alternatives (e.g., `date-fns` instead of `moment`).
4. Wrap non-critical interactive components in `next/dynamic`.
5. Measure INP impact in Chrome DevTools Performance profiler.

## Best Practices
- Debounce rapid user inputs (e.g. search bars) with 300ms delay.
- Use `useTransition` to mark heavy UI state updates as non-blocking.
- Avoid anonymous functions and object literals inside tight render loops when passing to memoized children.

## Anti-Patterns
- **Client Root Tagging**: Adding `'use client'` to `layout.tsx`, forcing the entire application tree to be sent to client JS bundle.
- **Synchronous Heavy Iteration**: Looping through 10,000 array items synchronously on button click, freezing the browser for 800ms.
- **Accidental Lodash / Moment Import**: Importing full `lodash` (70kB) for a single `capitalize` utility function.

## Validation Checklist
- [ ] Shared client First Load JS is under 100 kB (gzipped).
- [ ] Heavy components (charts, rich editors) are wrapped in `dynamic()`.
- [ ] No un-treeshaken barrel imports exist in client components.
- [ ] Long tasks (> 50ms) are eliminated from interaction handlers.

## Related Skills
- `skills/performance/core-web-vitals.md`
- `skills/frontend/react.md`
- `skills/frontend/nextjs.md`

## References
- Next.js Bundle Analyzer — https://www.npmjs.com/package/@next/bundle-analyzer
- web.dev: Optimize Long Tasks — https://web.dev/optimize-long-tasks/

## Last Reviewed
2026-09-15

# Core Web Vitals Engineering & Performance Budgets

## Purpose
Guarantees that web applications achieve top-tier Google Core Web Vitals scores, providing instantaneous loading, responsive user interactions, and rock-solid visual stability.

## When To Use
- In all web applications, landing pages, and e-commerce storefronts.
- When setting performance budgets, evaluating Lighthouse audits, or optimizing real-user metrics (RUM).

## When Not To Use
- For offline batch processing scripts or non-browser Node.js tasks.

## Core Principles
1. **The 3 Golden Metrics**:
   - **LCP (Largest Contentful Paint)**: **<= 2.5 seconds** (Measures perceived loading speed).
   - **INP (Interaction to Next Paint)**: **<= 200 milliseconds** (Measures interaction responsiveness; replaced FID).
   - **CLS (Cumulative Layout Shift)**: **<= 0.1** (Measures visual stability).
2. **Real-User Performance Over Synthetic Benchmarks**: Prioritize real device field data (Chrome UX Report / web-vitals RUM) over isolated, overpowered developer laptops.
3. **Hard Performance Budgets**: Set non-negotiable asset budgets:
   - Initial JavaScript bundle: **< 100 kB (gzipped)**.
   - Total page weight: **< 1.5 MB**.
   - Time to First Byte (TTFB): **< 800 ms**.

## Rules
- **Rule 1 (LCP Hero Element Optimization)**:
  The element responsible for LCP (usually a hero image or large heading) must:
  - Be discovered immediately in the raw HTML response (not fetched after client JS execution).
  - Use `priority` attribute if rendered via `next/image` to inject `<link rel="preload">`.
  - Avoid CSS background images (`background-image: url(...)`) for LCP candidates, as they suffer discovery delays.
- **Rule 2 (INP Latency Elimination)**:
  - Never run long JavaScript tasks (> 50ms) on the main thread during user interactions.
  - Break long computational tasks into chunks using `scheduler.yield()` or `setTimeout(..., 0)`.
  - Avoid heavy synchronous operations inside click/input event listeners; defer state updates using `startTransition`.
- **Rule 3 (CLS Layout Shift Elimination)**:
  - Every image, video, banner, and iframe must have explicit `width` and `height` aspect ratio attributes.
  - Dynamic content (ads, personalization widgets) must be injected into pre-allocated, fixed-height reserved containers (`min-h-[250px]`).
  - Use modern web fonts with `font-display: swap` and size-adjust metrics to prevent layout shifts during font swap.
- **Rule 4 (Web Vitals Telemetry)**:
  Track Core Web Vitals in production using the official `web-vitals` library:
  ```typescript
  import { onLCP, onINP, onCLS } from 'web-vitals';

  export function reportWebVitals() {
    onLCP((metric) => sendToAnalytics(metric));
    onINP((metric) => sendToAnalytics(metric));
    onCLS((metric) => sendToAnalytics(metric));
  }
  ```

## Decision Criteria
```text
IF LCP > 2.5s:
  Check TTFB (CDN cache / Edge SSR) -> Preload hero image -> Remove render-blocking resources.
IF INP > 200ms:
  Audit event handlers -> Defer non-critical state updates via useTransition -> Optimize component re-renders.
IF CLS > 0.1:
  Add explicit dimensions to media -> Pre-allocate skeleton containers -> Review font loading metric overrides.
```

## Recommended Workflow
1. Set baseline performance budget in project configuration.
2. Optimize Server Response Time (TTFB) via edge caching and streaming.
3. Preload critical fonts and hero assets in document `<head>`.
4. Wrap asynchronous components with explicit-dimension Suspense skeletons.
5. Measure via Chrome DevTools Performance panel and Lighthouse.

## Best Practices
- Keep third-party scripts (Google Tag Manager, analytics, chat widgets) loaded using Next.js `<Script strategy="lazyOnload">`.
- Avoid CSS `@import` inside stylesheets (causes serialized waterfall network requests).
- Enable HTTP/2 or HTTP/3 on the host CDN to multiplex parallel asset requests.

## Anti-Patterns
- **The Unsized Hero**: Placing a 4MB PNG hero image without explicit dimensions, destroying both LCP and CLS simultaneously.
- **Blocking the Main Thread**: Running heavy filtering or regex calculations inside an `onKeyDown` handler, causing 500ms INP lag.
- **Sudden Layout Insertion**: Inserting a cookie banner or promo ribbon at the top of the page without reserving space, pushing the whole DOM downward.

## Validation Checklist
- [ ] LCP is <= 2.5s on mobile 4G throttle.
- [ ] INP is <= 200ms during all user interactions.
- [ ] CLS is <= 0.1 across the entire user session.
- [ ] Hero image has `priority` attribute enabled.
- [ ] All media elements declare explicit aspect ratios.

## Related Skills
- `skills/performance/image-optimization.md`
- `skills/performance/javascript-performance.md`
- `checklists/performance-audit.md`

## References
- Google Chrome Web Vitals Guide — https://web.dev/vitals/
- Fast INP (Interaction to Next Paint) Guidelines — https://web.dev/optimize-inp/

## Last Reviewed
2026-09-15

# Web Vitals Optimization Cheat Sheet

## 1. LCP (Largest Contentful Paint) Fixes
- Preload the hero image: `<Image priority ... />`
- Use modern formats: Convert PNG/JPEG to WebP or AVIF.
- Serve via CDN Edge: Reduce TTFB below 800ms.
- Eliminate render-blocking CSS/JS in `<head>`.

---

## 2. INP (Interaction to Next Paint) Fixes
- Break long tasks: Keep main-thread tasks under 50ms.
- Defer non-critical state: Wrap complex re-renders in React `startTransition()`.
- Avoid heavy layout thrashing: Don't read DOM properties (`offsetHeight`) and immediately write styles in loops.
- Use Web Workers for CPU-heavy tasks.

---

## 3. CLS (Cumulative Layout Shift) Fixes
- Set explicit `width` and `height` (or aspect-ratio) on all images and videos.
- Reserve space for dynamic widgets and ads using `min-h-[200px]`.
- Use `font-display: swap` with `next/font` size-adjust overrides to prevent FOYT layout jumps.
- Avoid inserting content above existing content unless triggered by user interaction.

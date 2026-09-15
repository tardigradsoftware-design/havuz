# Core Web Vitals & Performance Audit Checklist

> **Stage**: Performance Optimization  
> **Mandatory**: Must be verified under simulated throttling.

---

## 1. Core Web Vitals Thresholds
- [ ] **LCP <= 2.5s** on mobile 4G network throttle.
- [ ] **INP <= 200ms** during all user clicks, toggles, and form inputs.
- [ ] **CLS <= 0.1** across entire session lifecycle.

## 2. Asset & Script Optimization
- [ ] Hero LCP image preloaded with `priority={true}` in `next/image`.
- [ ] All below-the-fold images lazy-loaded with modern formats (AVIF/WebP).
- [ ] Web fonts loaded with `font-display: swap` and zero FOIT/FOUT.
- [ ] Initial shared JavaScript bundle is < 100 kB (gzipped).
- [ ] Heavy third-party modules dynamically imported via `next/dynamic`.

---
*Sign-off: Performance Engineer / Lead Agent*

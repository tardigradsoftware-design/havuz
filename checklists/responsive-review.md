# Responsive & Multi-Device Audit Checklist

> **Stage**: Viewport Verification  
> **Mandatory**: Must be verified across device matrix.

---

## 1. Viewport Matrix
- [ ] Mobile Portrait (375px - iPhone SE): Zero horizontal overflow; touch targets >= 44x44px.
- [ ] Mobile Large (414px / 430px): Layout scales smoothly.
- [ ] Tablet (768px - iPad): Multi-column grids stack into 2-column or fluid layouts.
- [ ] Desktop (1280px / 1440px): Sidebar, header, and content maintain ergonomic max-widths (`max-w-7xl`).

## 2. Ergonomics & Interactions
- [ ] Primary touch actions positioned within easy thumb reach on mobile screens.
- [ ] Complex data tables degrade gracefully (stacked cards or horizontal scroll with sticky column).
- [ ] Mobile navigation sheets close cleanly upon link navigation.
- [ ] Safe area insets (`env(safe-area-inset-bottom)`) handled for iOS devices.

---
*Sign-off: Frontend Engineer / Lead Agent*

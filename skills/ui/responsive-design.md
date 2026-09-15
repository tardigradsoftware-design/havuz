# Responsive Design & Multi-Device Layout

## Purpose
Ensures web applications deliver flawless ergonomic experiences across mobile (375px), tablet (768px), standard desktop (1280px), and ultra-wide screens (1920px+) with zero horizontal overflow and optimal touch interactions.

## When To Use
- In every frontend layout, page template, navigation system, and component implementation.
- When adapting complex data tables, sidebars, and forms for small screens.

## When Not To Use
- For purely native desktop Electron windows locked to fixed aspect ratios (rare).

## Core Principles
1. **Mobile-First CSS**: Style the mobile viewport by default (`375px`), then progressively enhance with breakpoint prefixes (`md:`, `lg:`, `xl:`).
2. **Fluid Over Rigid**: Rely on fluid percentages, flexbox wrapping, CSS grid auto-fit/auto-fill, and `clamp()` typography rather than hardcoded pixel widths.
3. **Ergonomic Reach Zones**: Place primary touch actions within the thumb zone on mobile screens (bottom-anchored action bars, sticky sheets).

## Rules
- **Rule 1 (Breakpoint Standard)**: Standardize on standard Tailwind breakpoints:
  - `sm`: 640px (large smartphones, landscape)
  - `md`: 768px (tablets, small portables)
  - `lg`: 1024px (laptops, small monitors)
  - `xl`: 1280px (standard desktop displays)
  - `2xl`: 1536px (wide screens)
- **Rule 2 (Horizontal Overflow Prohibition)**: Zero horizontal scrolling is permitted on viewport root (`overflow-x: hidden` is a bandaid, not a cure; fix the offending element width).
- **Rule 3 (Touch Target Sizing)**: On touch screens (`max-md:`), all buttons, nav links, and form inputs must have a minimum interactive tap target of **44x44px** and minimum 8px separation between targets.
- **Rule 4 (Data Table Adaptation)**: Complex tables with > 4 columns must not be shrunk unreadably on mobile:
  - Option A: Wrap table in a container with explicit horizontal scroll (`overflow-x-auto`) and sticky first column.
  - Option B: Transform table rows into stacked mobile card views using CSS grid or responsive component swapping.

## Decision Criteria
```text
IF viewport < 768px:
  Collapse sidebar into mobile sheet/drawer; collapse desktop navbar into hamburger menu.
  Stack multi-column grids into single column (`grid-cols-1`).
IF viewport >= 768px and < 1024px:
  Use 2-column grid (`md:grid-cols-2`).
IF viewport >= 1024px:
  Use 3 or 4 column grids (`lg:grid-cols-3` or `lg:grid-cols-4`); expand permanent sidebar.
```

## Recommended Workflow
1. Set mobile viewport meta tag in HTML head: `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />`.
2. Code layout for 375px width without media queries.
3. Add `md:` prefixes for tablet layout optimizations.
4. Add `lg:` and `xl:` for desktop widescreen ergonomics.
5. Test against Chrome DevTools Device Mode: iPhone SE (375px), iPad (768px), and 1440px desktop.

## Best Practices
- Use `clamp(1.5rem, 4vw, 3rem)` for fluid hero typography that scales smoothly without jarring breakpoint jumps.
- Include `safe-area-inset` support for iOS notch/home indicator: `pb-[env(safe-area-inset-bottom)]`.
- Use `w-full max-w-7xl mx-auto` to prevent wide-screen stretching on 4K monitors.

## Anti-Patterns
- **Desktop-First Retrofitting**: Writing hundreds of lines of desktop CSS and trying to patch it with `@media (max-width: 640px)`.
- **Fixed Pixel Widths**: Specifying `width: 800px` or `min-width: 600px` on containers, immediately causing mobile horizontal clipping.
- **Hover-Only Interactions**: Hiding essential actions behind `:hover` states, rendering them inaccessible on touchscreens.

## Validation Checklist
- [ ] Does the page display cleanly at 375px width with zero horizontal overflow?
- [ ] Are all mobile touch targets at least 44x44px?
- [ ] Are mobile navbars and drawers tested with safe area insets?
- [ ] Do tables degrade gracefully on mobile screens?

## Related Skills
- `skills/ui/ui-design.md`
- `skills/ui/visual-hierarchy.md`
- `checklists/responsive-review.md`

## References
- MDN Responsive Design Guide — https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design
- Google Mobile-Friendly Guidelines

## Last Reviewed
2026-09-15

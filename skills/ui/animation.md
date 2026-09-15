# UI Animation, Micro-Interactions & Motion Physics

## Purpose
Defines motion guidelines and physics principles to make web interfaces feel responsive, tactile, and natural, without creating visual distractions or performance degradation.

## When To Use
- For state transitions (modals opening, accordions expanding, tab switching).
- For micro-interactions (button hover states, toggle switches, badge pulses).
- For scroll-driven reveal effects and page transitions.

## When Not To Use
- For constant looping decorative animations that distract from core tasks.
- When `prefers-reduced-motion: reduce` is detected in user system preferences.
- For data-dense enterprise tables where rapid scanning requires instant state changes.

## Core Principles
1. **Motion is Meaning, Not Decoration**: Animations must communicate cause-and-effect, spatial orientation, or system state.
2. **Speed & Snappiness**: UI transitions must be fast. Most interface animations should complete within **150ms to 300ms**. Anything longer than 400ms feels sluggish.
3. **Respect Reduced Motion**: Never force animations on users with vestibular motion disorders.

## Rules
- **Rule 1 (The Duration Matrix)**:
  - Micro-interactions (hover, active, focus): **100ms – 150ms**.
  - Small transitions (dropdowns, tooltips, popovers): **150ms – 200ms**.
  - Large transitions (modals, sheets, page transitions): **250ms – 300ms**.
  - Complex multi-step transitions: Max **400ms**.
- **Rule 2 (Easing Curves)**:
  - Entering the screen: `ease-out` (starts fast, settles gently). E.g., `cubic-bezier(0.16, 1, 0.3, 1)`.
  - Exiting the screen: `ease-in` (starts slow, accelerates away). E.g., `cubic-bezier(0.7, 0, 0.84, 0)`.
  - Toggle / In-place transformations: `ease-in-out` or standard spring physics (`stiffness: 300, damping: 30`).
  - Never use `linear` for UI movement (linear feels robotic and unnatural).
- **Rule 3 (Hardware Acceleration Guard)**:
  - Only animate `transform` (`translate`, `scale`, `rotate`) and `opacity`.
  - Never animate layout properties: `width`, `height`, `top`, `left`, `margin`, `padding` (causes layout reflow and frame drops).
- **Rule 4 (Reduced Motion Mandatory Fallback)**:
  Always provide a reduced motion media query:
  ```css
  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
      animation-duration: 0.01ms !important;
      animation-iteration-count: 1 !important;
      transition-duration: 0.01ms !important;
    }
  }
  ```
  In Framer Motion / Motion: Use `useReducedMotion()` to swap physical movement for simple opacity fades.

## Decision Criteria
```text
IF user prefers reduced motion:
  Eliminate transforms and physical translation; use instant transition or subtle opacity fade.
IF element is entering modal:
  Scale from 0.95 to 1.0 and fade opacity from 0 to 1 over 200ms with ease-out.
IF element is accordion:
  Use CSS grid-template-rows or Radix UI Accordion primitive with height transition.
```

## Recommended Workflow
1. Build the component fully functional without animation first.
2. Identify user feedback gaps (e.g., modal appears too abruptly).
3. Apply lightweight CSS transitions with Tailwind classes (`transition-all duration-200 ease-out`).
4. For complex gesture or layout animations, use Motion (`motiondivision/motion`).
5. Verify 60fps performance using Chrome DevTools Performance panel (zero red dropped-frame markers).

## Best Practices
- Use `will-change: transform` sparingly and only during active animations to avoid GPU memory leaks.
- Stagger lists of items subtly: `staggerChildren: 0.05` (50ms gap between items); never stagger so long that the user waits to see content.
- Ensure exit animations trigger cleanly before DOM node unmount (`AnimatePresence` in Motion).

## Anti-Patterns
- **The Rollercoaster**: Elements bouncing and flying across the screen from 800px away, inducing motion sickness.
- **Sluggish Interfaces**: Modals that take 700ms to fade in, making the app feel slow and unresponsive.
- **CPU Choking**: Animating `box-shadow` or `filter: blur()` continuously on 50 elements at once.

## Validation Checklist
- [ ] Are all transition durations between 150ms and 300ms?
- [ ] Are animations restricted to `transform` and `opacity`?
- [ ] Does the UI respect `prefers-reduced-motion: reduce`?
- [ ] Is frame rate maintained at a consistent 60fps during transitions?

## Related Skills
- `skills/ui/ui-design.md`
- `skills/ui/accessibility.md`
- `skills/performance/javascript-performance.md`

## References
- Motion (formerly Framer Motion) Documentation — https://motion.dev/
- Google Web Fundamentals: High Performance Animations — https://web.dev/animations/

## Last Reviewed
2026-09-15

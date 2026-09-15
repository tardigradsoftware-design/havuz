# Web Accessibility (A11y) & WCAG 2.2 AA Compliance

## Purpose
Guarantees that web applications are fully perceivable, operable, understandable, and robust for all users, including those relying on screen readers, keyboard navigation, voice control, and other assistive technologies.

## When To Use
- In every component, form, navigation structure, image, modal, and interactive control.
- During code reviews, automated CI audits, and pre-release verification.

## When Not To Use
- Never. Accessibility is a mandatory legal and ethical baseline for all commercial web software.

## Core Principles
1. **Semantic HTML First**: Use the platform's native HTML elements (`<button>`, `<a>`, `<nav>`, `<main>`, `<dialog>`). Never build fake interactive controls with `<div onClick={...}>`.
2. **Keyboard Operability**: Every action that can be performed with a mouse must be equally performable using only the keyboard (`Tab`, `Shift+Tab`, `Enter`, `Space`, `Escape`, `Arrow keys`).
3. **Information Equivalence**: Any visual status (color, icon, layout) must also be programmatically conveyed to screen readers.

## Rules
- **Rule 1 (Color Contrast Thresholds - WCAG 2.2 AA)**:
  - Normal text (< 18.6px bold or < 24px regular): **Minimum 4.5:1** contrast ratio against background.
  - Large text (>= 18.6px bold or >= 24px regular): **Minimum 3:1** contrast ratio.
  - UI components and graphical objects (borders, icons, focus rings): **Minimum 3:1** against adjacent colors.
- **Rule 2 (Visible Focus Indicators)**:
  - Never remove focus outlines with `outline: none` or `outline-0` without providing a visible, high-contrast replacement.
  - Standard focus ring: `focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-primary focus-visible:outline-none`.
- **Rule 3 (Images and Visual Media)**:
  - Informative images must have a concise, descriptive `alt` attribute.
  - Purely decorative images must have `alt=""` or `aria-hidden="true"`.
  - Icon-only buttons must have an explicit `aria-label` (e.g. `<button aria-label="Close dialog"><XIcon /></button>`).
- **Rule 4 (Form Accessibility)**:
  - Every form input must have a `<label>` associated via `htmlFor={id}` or `aria-labelledby`.
  - Error messages must be tied to inputs via `aria-invalid="true"` and `aria-describedby={errorId}`.
- **Rule 5 (Modal Focus Trapping & Restoration)**:
  - When a dialog opens, focus must move into the dialog, trap keyboard tab cycles within it, and close on `Escape`.
  - When closed, focus must be restored to the element that triggered it. Use Radix UI Dialog primitives to guarantee this.

## Decision Criteria
```text
IF element navigates to a new URL:
  Use semantic `<a>` tag or Next.js `<Link>`. NEVER use a button.
IF element triggers an action or mutates state:
  Use semantic `<button>` tag. NEVER use an `<a>` tag with href="#".
IF interactive widget is complex (Tabs, Accordion, Combobox):
  Use Radix UI Primitives / shadcn/ui; DO NOT invent custom ARIA roles from scratch.
```

## Recommended Workflow
1. Write semantic HTML tags before applying any CSS classes.
2. Add necessary `aria-*` attributes (`aria-expanded`, `aria-haspopup`, `aria-live="polite"`).
3. Test keyboard navigation: unplug mouse, navigate through page using only `Tab`, `Shift+Tab`, `Space`, `Enter`, and arrows.
4. Run automated accessibility scanner (`@axe-core/playwright` or Lighthouse a11y audit).
5. Verify with Screen Reader (macOS VoiceOver / NVDA).

## Best Practices
- Use `aria-live="polite"` on dynamic toast notifications so screen reader users hear asynchronous updates without interruption.
- Ensure form inputs specify proper `autocomplete` attributes (`email`, `current-password`, `tel`, `address-line1`).
- Avoid auto-playing video or audio without user consent.

## Anti-Patterns
- **The Clickable Div**: `<div onClick={handleClick}>Submit</div>` (Inaccessible to keyboard, missing role, missing focus ring).
- **Hidden Focus Rings**: Applying `outline: none` because the designer disliked the browser default focus ring.
- **Color-Only Status**: Using only red text without an error icon or "Error:" prefix, locking out color-blind users.

## Validation Checklist
- [ ] Automated `@axe-core` scan passes with 0 violations.
- [ ] Every interactive element is reachable and operable via keyboard alone.
- [ ] Visible focus ring is obvious on every interactive element.
- [ ] Text contrast ratios satisfy WCAG 2.2 AA (4.5:1 for body text).
- [ ] All icon buttons have descriptive `aria-label` attributes.

## Related Skills
- `skills/ui/typography.md`
- `skills/testing/accessibility-testing.md`
- `checklists/accessibility-audit.md`

## References
- W3C Web Content Accessibility Guidelines (WCAG) 2.2 — https://www.w3.org/TR/WCAG22/
- W3C WAI-ARIA Authoring Practices Guide (APG) — https://www.w3.org/WAI/ARIA/apg/

## Last Reviewed
2026-09-15

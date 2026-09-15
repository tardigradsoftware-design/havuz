# Accessibility (WCAG 2.2 AA) Audit Checklist

> **Stage**: Assistive Tech & Usability  
> **Mandatory**: Must be verified before production sign-off.

---

## 1. Automated & Semantic Checks
- [ ] Automated `@axe-core/playwright` scan completes with 0 violations.
- [ ] Semantic HTML tags used for all interactive elements (`<button>`, `<a>`, `<dialog>`).
- [ ] Text contrast ratios satisfy WCAG 2.2 AA (>= 4.5:1 body, >= 3.0:1 headings).
- [ ] Icon-only buttons include descriptive `aria-label` attributes.

## 2. Keyboard & Screen Reader Verification
- [ ] Every interactive element is reachable and operable via keyboard alone.
- [ ] Focus rings are clearly visible on keyboard navigation (`focus-visible:ring-2`).
- [ ] Modal dialogs trap focus and close on `Escape`.
- [ ] Asynchronous status updates and notifications announced via `aria-live`.

---
*Sign-off: Accessibility Lead / Lead Agent*

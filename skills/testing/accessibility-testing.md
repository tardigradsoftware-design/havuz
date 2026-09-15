# Automated Accessibility Testing & Assistive Tech Verification

## Purpose
Integrates automated accessibility scanning into CI pipelines and provides deterministic protocols to verify keyboard operability, screen reader announcements, and WCAG 2.2 AA compliance.

## When To Use
- In every component test, page integration test, and pre-release audit.
- When validating interactive dialogs, menus, forms, and custom widgets.

## When Not To Use
- Never. Accessibility testing is mandatory for all user interfaces.

## Core Principles
1. **Automate the Detectable, Audit the Rest**: Automated tools (axe-core) catch ~40–50% of accessibility violations (contrast, missing labels, invalid ARIA). Manual keyboard and screen reader checks are mandatory for the rest.
2. **Zero Serious or Critical Violations**: Any violation tagged `serious` or `critical` by axe-core fails the test suite.
3. **Focus Visibility & Logical Flow**: Tab order must strictly follow reading order, and focus indicators must remain visible at all times.

## Rules
- **Rule 1 (Automated axe-core Scan Standard)**:
  Every critical page and modal must pass an automated axe-core scan:
  ```typescript
  import { test, expect } from '@playwright/test';
  import AxeBuilder from '@axe-core/playwright';

  test('Dashboard page passes WCAG 2.2 AA accessibility audit', async ({ page }) => {
    await page.goto('/dashboard');
    await page.waitForLoadState('networkidle');

    const accessibilityScanResults = await new AxeBuilder({ page })
      .withTags(['wcag2a', 'wcag2aa', 'wcag21aa', 'wcag22aa'])
      .analyze();

    expect(accessibilityScanResults.violations).toEqual([]);
  });
  ```
- **Rule 2 (Keyboard Trap Verification)**:
  When testing modals, sheets, or dialogs, verify that pressing `Tab` repeatedly cycles only through focusable elements inside the modal and never escapes to the underlying page.
- **Rule 3 (Escape Key Dismissal)**:
  Verify that pressing `Escape` closes open popovers, dropdowns, and modals, returning focus to the triggering element.
- **Rule 4 (Screen Reader Announcement Assertions)**:
  Dynamic status messages must be verified for presence in live regions:
  ```typescript
  await expect(page.locator('[aria-live="polite"]')).toHaveText('3 items added to cart');
  ```

## Decision Criteria
```text
IF axe-core detects color contrast violation:
  Increase text weight or adjust foreground/background token to achieve >= 4.5:1.
IF interactive element is not reachable by Tab:
  Replace with native <button> or add tabIndex={0} and keyboard event handlers.
IF modal allows focus to reach background page:
  Wrap in Radix UI Dialog or set aria-modal="true" with focus-trap-react.
```

## Recommended Workflow
1. Run `@axe-core/playwright` as part of regular E2E suite.
2. Perform manual keyboard run-through: Navigate using `Tab`, `Shift+Tab`, `Space`, `Enter`, and arrows.
3. Verify visible focus outline on every interactive element.
4. Test with VoiceOver (macOS) or NVDA (Windows) to verify announcement clarity.
5. Confirm zero accessibility violations before merge.

## Best Practices
- Run accessibility tests on both closed and open states of interactive dialogs and menus.
- Include form validation error states in accessibility tests to verify `aria-invalid` and `aria-describedby` wiring.
- Exclude false positives cautiously and only with documented justification in `AxeBuilder().exclude(...)`.

## Anti-Patterns
- **The "We Use Radix So We're Good" Myth**: Assuming an accessible library automatically makes your composition accessible (e.g. forgetting to add labels or creating low contrast).
- **Skipping Keyboard Navigation**: Testing only with automated scanners and never pressing the `Tab` key.
- **Hidden Focus Rings**: Removing focus outlines with `outline-none` without providing `focus-visible:ring`.

## Validation Checklist
- [ ] axe-core audit completes with 0 violations.
- [ ] Every button and link is reachable and triggerable via keyboard.
- [ ] Modal dialogs trap focus and close on `Escape`.
- [ ] Dynamic asynchronous status updates are announced via `aria-live`.

## Related Skills
- `skills/ui/accessibility.md`
- `skills/testing/playwright.md`
- `checklists/accessibility-audit.md`

## References
- Axe-core Official Documentation — https://github.com/dequelabs/axe-core
- Playwright Accessibility Testing Guide — https://playwright.dev/docs/accessibility-testing

## Last Reviewed
2026-09-15

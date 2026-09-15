# Visual Regression Testing & Screenshot Diffing

## Purpose
Detects unintentional visual regressions, broken CSS layouts, typography clipping, and unintended component styling shifts across multiple viewports and operating systems.

## When To Use
- When refactoring global stylesheets, Tailwind configurations, or design system tokens.
- When verifying component library primitives and high-conversion landing page layouts.

## When Not To Use
- For rapidly changing dynamic data screens (e.g., live stock feeds, real-time activity logs) unless dynamic values are properly masked.

## Core Principles
1. **Pixel-Level Precision**: Visual tests compare pixel-by-pixel screenshots against audited baseline images.
2. **Deterministic Rendering**: Disable animations, mock system clocks, and freeze dynamic content before capturing visual snapshots.
3. **Cross-Viewport Coverage**: Test across mobile (375px), tablet (768px), and desktop (1280px) viewports.

## Rules
- **Rule 1 (Playwright Visual Comparison Standard)**:
  Capture and assert screenshots using Playwright's built-in snapshot matcher:
  ```typescript
  test('Landing page visual snapshot matches baseline', async ({ page }) => {
    await page.goto('/');
    // Freeze animations and dynamic elements
    await page.addStyleTag({
      content: `*, *::before, *::after {
        animation: none !important;
        transition: none !important;
      }`,
    });
    // Wait for fonts to load
    await page.evaluate(async () => document.fonts.ready);
    // Visual snapshot comparison with tight threshold
    await expect(page).toHaveScreenshot('landing-page-desktop.png', {
      maxDiffPixelRatio: 0.02, // Max 2% pixel tolerance
    });
  });
  ```
- **Rule 2 (Dynamic Element Masking)**:
  Mask volatile UI elements (avatars, timestamps, random numbers) so they do not trigger false positive visual failures:
  ```typescript
  await expect(page).toHaveScreenshot({
    mask: [page.getByTestId('dynamic-timestamp'), page.getByRole('img', { name: 'User avatar' })],
  });
  ```
- **Rule 3 (Component-Level Isolation Testing)**:
  Test atomic UI components (Button states, Modal dialogs, Dropdowns) in isolation rather than relying solely on full-page screenshots.
- **Rule 4 (Baseline Update Protocol)**:
  Never run `npx playwright test --update-snapshots` blindly. Every baseline diff must be inspected and approved in a code review pull request.

## Decision Criteria
```text
IF page contains CSS animations or transitions:
  Disable CSS animations via style injection before taking snapshot.
IF font rendering varies between Linux CI and local macOS:
  Run visual snapshot tests inside official Playwright Docker container: mcr.microsoft.com/playwright.
IF element diff is due to intentional redesign:
  Review diff image -> Update baseline screenshot -> Commit new baseline to Git.
```

## Recommended Workflow
1. Identify visual-critical components and marketing layouts.
2. Write Playwright visual test with viewport configurations.
3. Generate initial baseline screenshots: `npx playwright test --update-snapshots`.
4. Run regression suite on pull requests in CI Docker container.
5. Review visual diff reports if changes are flagged.

## Best Practices
- Run visual regression tests in a containerized environment (Docker) to eliminate cross-platform font rendering discrepancies.
- Test both Light and Dark mode themes.
- Test hover and focus states using `locator.hover()` before snapshot capture.

## Anti-Patterns
- **Unmasked Timestamps**: Testing a page that displays "Last updated: 3 seconds ago", causing tests to fail on every single run.
- **Ignored Differences**: Raising the `maxDiffPixelRatio` to 0.20 (20%) just to make tests pass, rendering visual testing useless.
- **Testing Animations Mid-Flight**: Taking a screenshot 100ms into a 300ms fade-in transition, capturing random semi-transparent pixels.

## Validation Checklist
- [ ] Fonts are fully loaded before capturing snapshots (`document.fonts.ready`).
- [ ] CSS animations are disabled during capture.
- [ ] Dynamic timestamps and avatars are masked.
- [ ] Visual tests run inside consistent CI Docker environments.

## Related Skills
- `skills/testing/playwright.md`
- `skills/ui/responsive-design.md`
- `checklists/ui-review.md`

## References
- Playwright Visual Comparisons Guide — https://playwright.dev/docs/test-snapshots
- Pixelmatch Image Comparison Library — https://github.com/mapbox/pixelmatch

## Last Reviewed
2026-09-15

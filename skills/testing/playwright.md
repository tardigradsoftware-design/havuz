# Playwright Automation, Fixtures & Web-First Assertions

## Purpose
Establishes official Microsoft Playwright best practices, Page Object Models, auto-waiting assertions, parallel execution, and debugging workflows.

## When To Use
- In all browser automation, end-to-end testing, visual regression testing, and CI pipeline validation.
- When simulating authentic user clicks, form typing, file uploads, and network responses.

## When Not To Use
- For isolated pure JavaScript utility functions where Vitest unit tests execute 100x faster.

## Core Principles
1. **Web-First Assertions**: Always use Playwright's `expect(locator).toBeVisible()` assertions, which automatically wait and retry until condition is met or timeout occurs.
2. **Page Object Model (POM)**: Encapsulate page UI structures into reusable Page Objects to insulate tests from visual DOM refactors.
3. **Hermetic Test Isolation**: Every test must run in complete isolation with its own browser context and independent test data.

## Rules
- **Rule 1 (The Web-First Assertion Rule)**:
  Never write manual sleeps or pollers. Always use web-first matchers:
  ```typescript
  // CORRECT (Auto-waits up to 5000ms):
  await expect(page.getByRole('alert')).toHaveText('Project created successfully');

  // INCORRECT (Flaky, races with DOM rendering):
  // const text = await page.getByRole('alert').textContent();
  // expect(text).toBe('Project created successfully');
  ```
- **Rule 2 (Page Object Model Standard)**:
  Structure complex pages into Page Objects:
  ```typescript
  import { type Locator, type Page, expect } from '@playwright/test';

  export class LoginPage {
    readonly page: Page;
    readonly emailInput: Locator;
    readonly passwordInput: Locator;
    readonly submitButton: Locator;

    constructor(page: Page) {
      this.page = page;
      this.emailInput = page.getByLabel('Email address');
      this.passwordInput = page.getByLabel('Password');
      this.submitButton = page.getByRole('button', { name: 'Sign in' });
    }

    async goto() {
      await this.page.goto('/login');
    }

    async login(email: string, pass: string) {
      await this.emailInput.fill(email);
      await this.passwordInput.fill(pass);
      await this.submitButton.click();
    }
  }
  ```
- **Rule 3 (Authentication State Caching)**:
  Do not log in via the UI before every single test. Use `global-setup.ts` to log in once, save `storageState`, and provide it to all worker contexts:
  ```typescript
  // playwright.config.ts
  use: {
    storageState: 'playwright/.auth/user.json',
  }
  ```
- **Rule 4 (Trace Viewer on Failure)**:
  Always configure trace recording on first retry in CI:
  `trace: 'on-first-retry'`. This captures full DOM snapshots, console logs, and network traffic for effortless debugging.

## Decision Criteria
```text
IF waiting for an element to appear:
  Use await expect(locator).toBeVisible(); NEVER use page.waitForTimeout().
IF mocking external third-party API:
  Use page.route('**/api/v1/stripe/**', route => route.fulfill({ status: 200 }));
IF testing cross-browser compatibility:
  Configure projects matrix for Chromium, Firefox, WebKit, and Mobile Safari.
```

## Recommended Workflow
1. Initialize Playwright: `npm init playwright@latest`.
2. Configure `playwright.config.ts` with baseURL, trace, and timeouts.
3. Build Page Objects in `e2e/pages/`.
4. Write test scenarios in `e2e/tests/`.
5. Run tests locally: `npx playwright test --ui`.
6. Inspect trace files if any test fails: `npx playwright show-trace trace.zip`.

## Best Practices
- Use `locator.fill()` instead of `locator.type()` for faster, reliable text entry.
- Set reasonable timeouts: default 30s test timeout, 5s assertion timeout.
- Use Playwright Codegen (`npx playwright codegen localhost:3000`) for rapid test scaffolding.

## Anti-Patterns
- **Hardcoded Sleep Calls**: `await page.waitForTimeout(3000)` (Fragile, slows down CI suites).
- **XPath Locators**: `page.locator('//div[@id="main"]/div[2]/button[1]')` (Breaks immediately when DOM wraps in a div).
- **Shared Mutable State**: Multiple tests modifying the same user record simultaneously, creating race conditions.

## Validation Checklist
- [ ] All assertions use web-first `expect(locator)` matchers.
- [ ] Selectors rely on user-facing roles and labels.
- [ ] Trace viewer is configured for failure analysis.
- [ ] Tests run successfully in parallel headless mode.

## Related Skills
- `skills/testing/e2e.md`
- `skills/testing/visual-testing.md`
- `checklists/testing-audit.md`

## References
- Official Playwright Documentation — https://playwright.dev/
- Playwright Best Practices — https://playwright.dev/docs/best-practices

## Last Reviewed
2026-09-15

# End-to-End User Journey Testing & Resilient Workflows

## Purpose
Simulates complete, authentic user journeys across multiple pages, network mutations, and state transitions to verify real-world application reliability.

## When To Use
- For mission-critical workflows: Onboarding, Checkout, Subscription billing, Account deletion, Data export.
- When validating integration across frontend, backend, database, and third-party webhook handlers.

## When Not To Use
- For exhaustive combinatorial branch testing of edge-case calculation formulas (use Vitest unit tests).

## Core Principles
1. **Test Like a User**: Automate actions through authentic user gestures (clicking buttons, typing into fields, scrolling, pressing keyboard keys).
2. **Resilient User Journeys**: Model tests around real user intent rather than internal technical steps.
3. **Zero Test Contamination**: Each test run must clean up or isolate its created entities to avoid polluting test databases.

## Rules
- **Rule 1 (The Golden Path Contract)**:
  Every product feature must maintain an automated test executing the end-to-end "Golden Path" from initiation to completion:
  ```typescript
  test('New customer completes checkout successfully', async ({ page }) => {
    // 1. Visit catalog
    await page.goto('/products');
    
    // 2. Add product to cart
    await page.getByRole('link', { name: 'Ergonomic Keyboard' }).click();
    await page.getByRole('button', { name: 'Add to Cart' }).click();
    
    // 3. Open cart and proceed
    await expect(page.getByRole('dialog', { name: 'Shopping Cart' })).toBeVisible();
    await page.getByRole('button', { name: 'Checkout' }).click();
    
    // 4. Fill customer details
    await page.getByLabel('Email address').fill('buyer@example.com');
    await page.getByLabel('Shipping address').fill('123 Market Street');
    await page.getByRole('button', { name: 'Pay and Place Order' }).click();
    
    // 5. Verify confirmation
    await expect(page.getByRole('heading', { name: 'Thank you for your order!' })).toBeVisible();
    await expect(page.getByText('Order confirmation sent to buyer@example.com')).toBeVisible();
  });
  ```
- **Rule 2 (Network Mocking for External Vendors)**:
  Mock third-party external networks (Stripe, Twilio, Sendgrid) to ensure tests do not depend on external internet uptime or incur real API charges.
- **Rule 3 (Data Seeding & Teardown)**:
  Seed database records directly via ORM/API before running tests; never spend 20 UI steps creating prerequisite records that are not the subject of the test.

## Decision Criteria
```text
IF test requires an existing project to test project settings:
  Seed project directly via database fixture; DO NOT navigate through project creation wizard.
IF testing third-party OAuth provider:
  Bypass provider login using pre-generated session storage state or mock auth handler.
IF test fails in CI but passes locally:
  Inspect Playwright trace artifact; check for race conditions in dynamic rendering.
```

## Recommended Workflow
1. Outline critical business journeys.
2. Create database fixtures to seed necessary prerequisite data.
3. Implement test using resilient role-based locators.
4. Add assertions verifying both visual UI confirmation and database persistence.
5. Run against local headless Chromium, Firefox, and WebKit.

## Best Practices
- Group related journeys into descriptive `test.describe('Subscription Management', () => { ... })` blocks.
- Test error recovery: verify what happens when payment fails or input validation rejects.
- Keep tests independent so they can run in parallel in any arbitrary order.

## Anti-Patterns
- **Daisy-Chaining Tests**: Having Test B depend on data created by Test A (If Test A fails, all subsequent tests fail cascade).
- **UI-Heavy Setup**: Spending 30 clicks creating test users and organizations before testing a 1-click feature.
- **Flaky Dynamic Timestamps**: Hardcoding expectations like `expect(date).toBe('Today')` that fail across midnight runs.

## Validation Checklist
- [ ] Golden paths for core business features are covered.
- [ ] Tests run completely independently with zero shared mutable data.
- [ ] Third-party API payments/emails are mocked safely.
- [ ] Tests pass consistently across 10 consecutive runs.

## Related Skills
- `skills/testing/playwright.md`
- `skills/testing/testing-strategy.md`
- `skills/ecommerce/checkout.md`

## References
- Martin Fowler on User Journey Tests — https://martinfowler.com/bliki/UserJourneyTest.html
- Playwright Network Mocking Guide — https://playwright.dev/docs/network

## Last Reviewed
2026-09-15

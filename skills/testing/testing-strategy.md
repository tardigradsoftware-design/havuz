# Comprehensive Testing Strategy & Quality Pyramid

## Purpose
Establishes a balanced, efficient, and reliable testing strategy that maximizes confidence while minimizing maintenance overhead and test execution latency.

## When To Use
- In all software projects when planning test architectures, selecting test tools, and configuring CI gates.
- When establishing coverage targets and balancing unit, integration, and E2E test suites.

## When Not To Use
- For throwaway spike prototypes where code will be completely discarded within 24 hours.

## Core Principles
1. **The Practical Quality Pyramid**:
   - **Unit Tests (Fast / High Volume)**: Test pure business logic, calculations, date formatters, and Zod schemas (Vitest).
   - **Integration Tests (Medium / Balanced)**: Test component interactions, form validation cycles, and API route handlers (Vitest + React Testing Library).
   - **End-to-End Tests (Critical / High Confidence)**: Test realistic user journeys in headless browsers across real database states (Playwright).
2. **Test User-Visible Behavior, Not Implementation Details**: Avoid asserting internal component state, private variables, or specific CSS classes. Test what the user sees and does.
3. **Deterministic Reliability**: Zero tolerance for flaky tests. A test that fails intermittently must be fixed or removed immediately.

## Rules
- **Rule 1 (Tooling Standard)**:
  - Unit & Integration Runner: **Vitest** (Native ESM, fast watch mode, Jest compatible).
  - End-to-End & Browser Automation: **Playwright** (Cross-browser, auto-waiting, trace viewer).
  - Accessibility Scanner: **@axe-core/playwright**.
- **Rule 2 (Selector Resilience Hierarchy)**:
  When targeting DOM elements in tests, use the priority ladder:
  1. `page.getByRole('button', { name: 'Submit' })` (Semantic & Accessible)
  2. `page.getByLabel('Email address')` (Form Inputs)
  3. `page.getByPlaceholder('Search products...')`
  4. `page.getByText('Welcome back')`
  5. `page.getByTestId('checkout-card')` (Last resort when semantic queries fail)
  - **Banned**: CSS tag/class selectors (`page.locator('.btn-primary > div')`).
- **Rule 3 (Critical Flow E2E Coverage)**:
  Every production web application must maintain automated E2E tests covering:
  - User Registration & Authentication Flow.
  - Primary Core Journey (e.g., E-Commerce Checkout, SaaS Project Creation).
  - Destructive Action & Confirmation Workflow.
- **Rule 4 (CI Gate Enforcement)**:
  CI pipelines must block pull requests on:
  - TypeScript compilation failure (`tsc --noEmit`).
  - Unit/Integration test failures (`npm run test`).
  - Playwright critical journey test failures (`npm run test:e2e`).

## Decision Criteria
```text
IF testing pure logic / calculation / reducer:
  Write Vitest unit test; execution < 5ms.
IF testing complex multi-page workflow with database:
  Write Playwright E2E test.
IF testing accessible user interface component:
  Write Vitest component test with @testing-library/react and @axe-core.
```

## Recommended Workflow
1. Write unit tests alongside domain logic (`*.test.ts`).
2. Implement feature until unit tests pass.
3. Scaffold Playwright E2E journey simulating real user navigation.
4. Run tests with trace recording enabled.
5. Review test execution time; optimize slow tests.

## Best Practices
- Run tests in parallel across worker threads.
- Isolate test state by seeding unique database records per test worker.
- Use Playwright storage state (`storageState.json`) to authenticate once and reuse credentials across 50 tests.

## Anti-Patterns
- **The Inverted Ice Cream Cone**: Having 0 unit tests and 500 brittle, slow E2E tests that take 45 minutes to run.
- **Testing Implementation Details**: Asserting `expect(wrapper.state('isOpen')).toBe(true)` instead of checking if the dialog is visible in the DOM.
- **Arbitrary Sleep Statements**: Using `page.waitForTimeout(5000)` instead of Playwright web-first auto-waiting assertions.

## Validation Checklist
- [ ] Critical user journeys are covered by automated E2E tests.
- [ ] Selectors use accessible queries (`getByRole`, `getByLabel`).
- [ ] Zero arbitrary timeouts (`sleep` / `waitForTimeout`) exist in test files.
- [ ] All tests execute deterministically in CI.

## Related Skills
- `skills/testing/playwright.md`
- `skills/testing/e2e.md`
- `skills/core/verification.md`

## References
- Google Testing Blog: Just Say No to More End-to-End Tests
- Testing Library Guiding Principles — https://testing-library.com/docs/guiding-principles/

## Last Reviewed
2026-09-15

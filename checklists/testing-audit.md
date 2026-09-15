# Automated Test Suite Verification Checklist

> **Stage**: Quality Assurance & Regression  
> **Mandatory**: Must pass 100% in CI.

---

## 1. Suite Execution
- [ ] TypeScript typecheck passes with 0 errors (`npx tsc --noEmit`).
- [ ] Unit and integration tests pass 100% (`npm run test`).
- [ ] Playwright E2E tests pass 100% across Chromium, Firefox, and WebKit.
- [ ] Zero arbitrary timeouts (`sleep` / `waitForTimeout`) in test suites.

## 2. Journey Coverage
- [ ] Critical business golden paths covered by end-to-end browser journeys.
- [ ] Form validation edge cases (empty fields, malformed emails) tested.
- [ ] Destructive actions protected by confirmation checks in tests.
- [ ] Network mocks isolate tests from external API downtime.

---
*Sign-off: QA Lead / Lead Agent*

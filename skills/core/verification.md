# Deterministic Verification & Validation Protocols

## Purpose
Establishes the mandatory mechanical, automated, and observable protocols required to prove that software changes work flawlessly before sign-off.

## When To Use
- After code implementation and self-critique.
- Prior to committing, pushing, or reporting completion to the user.
- When validating bug fixes to prove regression elimination.

## When Not To Use
- Pure exploratory ideation or initial requirement drafting before files are created.

## Core Principles
1. **Evidence Over Assertion**: Never claim something works without running the tool that proves it works.
2. **Deterministic Assertions**: Verification must rely on exit codes (`0`), machine-parseable test suites, and strict compiler checks.
3. **End-to-End Integrity**: Unit test passage alone is insufficient; critical user flows must be verified in simulated browser or runtime environments.

## Rules
- **Rule 1 (Compiler Zero Tolerance)**: Every verification pass must run `npx tsc --noEmit` (or framework typecheck equivalent). Exit code must equal `0`.
- **Rule 2 (Test Execution Proof)**: If a test runner (`vitest`, `jest`, `playwright`) exists in the project, run the relevant test suite and observe pass results.
- **Rule 3 (Visual / Runtime Smoke Test)**: For frontend changes, inspect the DOM structure, verify responsive layout behavior at 375px and 1280px, and ensure no console errors are thrown during component mount.
- **Rule 4 (Build Verification)**: For full-stack and Next.js applications, run `npm run build` to verify route manifests, static generation, and bundle size constraints before final sign-off.

## Decision Criteria
```text
IF typecheck fails:
  STOP. Do not proceed to testing. Fix type definitions.
IF unit/integration tests fail:
  STOP. Identify regression. Do not modify test expectations unless requirements changed.
IF build fails with SSR/Hydration error:
  STOP. Check for window/document access in Server Components or mismatched server/client markup.
```

## Recommended Workflow
1. **Type & Static Analysis Check**:
   ```bash
   npx tsc --noEmit
   npm run lint
   ```
2. **Unit / Integration Test Suite**:
   ```bash
   npm run test
   ```
3. **Production Build Simulation**:
   ```bash
   npm run build
   ```
4. **End-to-End / Accessibility Automated Audit**:
   ```bash
   npx playwright test
   ```
5. **Log Verification Evidence**: Record exact test counts, pass rates, and build outputs in final deliverable summary.

## Best Practices
- Isolate test runs to the changed domain during iteration, then run the full suite at final release.
- Include boundary value tests (zero items, maximum characters, invalid emails, network timeouts).
- Treat flaky tests as failed tests; fix race conditions immediately.

## Anti-Patterns
- **"It Should Work" Syndrome**: Declaring victory based on looking at code without executing verification commands.
- **Test Deletion**: Removing or skipping failing tests to pass CI rather than fixing the underlying bug.
- **Blind Flagging**: Adding `@ts-nocheck` or `eslint-disable` to bypass verification checks.

## Validation Checklist
- [ ] TypeScript compiler passed with zero errors.
- [ ] Linter passed with zero warnings.
- [ ] Automated tests executed and passed 100%.
- [ ] Production build completed without bundle generation errors.

## Related Skills
- `skills/core/self-critique.md`
- `skills/testing/playwright.md`
- `skills/testing/testing-strategy.md`

## References
- Continuous Integration Standards — Martin Fowler
- Playwright Verification Guide — https://playwright.dev/docs/intro

## Last Reviewed
2026-09-15

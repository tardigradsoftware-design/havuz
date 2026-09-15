# Code Maintainability, Refactoring & Technical Debt Governance

## Purpose
Maintains high codebase velocity and health over years of development by enforcing clean code standards, automated refactoring safety nets, and systematic technical debt reduction.

## When To Use
- During every feature build, code review, and periodic engineering cleanup cycle.
- When evaluating legacy code for modernization or decommissioning deprecated patterns.

## When Not To Use
- Never. Maintainability is the bedrock of enduring software systems.

## Core Principles
1. **The Boy Scout Rule**: Always leave the codebase cleaner than you found it. If you touch a file with an outdated pattern, clean it up as part of your milestone.
2. **Self-Documenting Code**: Code must be readable without prose comments. Use clear naming, descriptive variables, and small functions. Comments should only explain *why* an unusual decision was made.
3. **Dead Code Elimination**: Code that is not executed is a liability. Delete dead code, unused dependencies, and orphaned CSS immediately; Git history preserves everything.

## Rules
- **Rule 1 (The Function Complexity Cap)**:
  - Cyclomatic complexity per function must not exceed **10**.
  - Functions should perform one clear task. If a function contains 4 levels of nested `if/else` statements, extract sub-routines.
- **Rule 2 (No Commented-Out Dead Code)**:
  Never commit commented-out code blocks (`// const oldLogic = ...`). Rely on Git history to retrieve past implementations.
- **Rule 3 (Meaningful Identifier Naming)**:
  - Variables: Descriptive nouns (`userPreferences`, `activeSubscription`).
  - Booleans: Must begin with auxiliary verb (`isOpen`, `hasAccess`, `canEdit`, `isLoading`).
  - Functions: Imperative verbs (`fetchProject`, `calculateTax`, `formatCurrency`).
  - Avoid single-letter abbreviations (`u`, `p`, `x`, `arr`) except in standard mathematical or short loop counters.
- **Rule 4 (Automated Hygiene Gates)**:
  Automate code hygiene via tools:
  - ESLint / Biome for linting and unused variable detection (`no-unused-vars`).
  - Prettier for consistent code formatting.
  - Knip or depcheck for finding unused exports and orphaned dependencies.

## Decision Criteria
```text
IF a function exceeds 50 lines:
  Evaluate for refactoring into smaller, single-purpose helper functions.
IF an npm package is no longer referenced in the codebase:
  Run npm uninstall immediately; remove from package.json.
IF complex business logic requires explanation:
  Add concise comment explaining the non-obvious WHY (e.g. legal regulation or browser quirk).
```

## Recommended Workflow
1. Run automated code hygiene check: `npx knip` to identify dead code.
2. Run linter and typecheck: `npm run lint` and `npx tsc --noEmit`.
3. Refactor messy sections using verified automated tests as a safety harness.
4. Verify all tests continue to pass after refactoring.
5. Create clean, atomic Git commits with clear commit messages.

## Best Practices
- Prefer pure functions with no side effects wherever possible.
- Favor early returns (`guard clauses`) over deep nested conditional pyramids.
- Standardize on immutable data transformations (`map`, `filter`, spread) rather than in-place mutations.

## Anti-Patterns
- **The Magic Number / String**: Hardcoding `if (status === 3)` or `timeout = 86400` without declaring named constants.
- **The Zombie Comment**: Comments describing what code did three years ago that no longer match the actual implementation.
- **The "I'll Fix It Later" Trap**: Writing messy code with a `// TODO: refactor this terrible hack` comment that stays for 4 years.

## Validation Checklist
- [ ] No dead, commented-out code blocks exist.
- [ ] Variable and function names are self-explanatory and descriptive.
- [ ] Functions use guard clauses to prevent deep nesting.
- [ ] Unused dependencies and orphaned exports have been removed.

## Related Skills
- `skills/core/self-critique.md`
- `skills/core/verification.md`
- `skills/architecture/frontend-architecture.md`

## References
- Clean Code: A Handbook of Agile Software Craftsmanship — Robert C. Martin
- Refactoring: Improving the Design of Existing Code — Martin Fowler

## Last Reviewed
2026-09-15

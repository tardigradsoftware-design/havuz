# Structured Task Planning & Decomposition

## Purpose
Guides AI coding agents in decomposing complex, ambiguous web development prompts into atomic, verifiable, and executable milestones before touching any code.

## When To Use
- At the start of every non-trivial development assignment.
- When scaffolding new features, refactoring major subsystems, or building full applications from scratch.
- When managing multi-step migrations (e.g., Pages Router to App Router, CSS to Tailwind).

## When Not To Use
- For trivial, single-line typo corrections or isolated bug patches where immediate resolution is evident.

## Core Principles
1. **Spec First, Code Second**: An engineer never writes code without defining the success criteria, boundaries, and test assertions upfront.
2. **Atomic Milestones**: Decompose work into discrete increments that leave the repository in a compiling, testable state at every step.
3. **Dependency Ordering**: Scaffolding and shared contracts (types, schemas) must precede business logic and UI presentation.

## Rules
- **Rule 1 (Explicit Plan Generation)**: Before modifying files on multi-file tasks, formulate a numbered implementation plan detailing:
  1. Contracts & Schemas to create/update.
  2. Server/Backend logic & API endpoints.
  3. Client UI components & state integration.
  4. Test cases & validation commands.
- **Rule 2 (No Orphan Milestones)**: Every milestone must terminate with a verifiable command (e.g., `tsc --noEmit`, test execution, or curl assertion).
- **Rule 3 (Milestone Scope Cap)**: A single milestone must not exceed 5 closely related files. If a milestone touches 10+ files, split it into structural vs. implementation phases.

## Decision Criteria
```text
IF task is a full application / feature:
  Decompose into: Data Layer -> API/Service -> UI Primitives -> Page Assembly -> Testing.
ELSE IF task is a bug fix:
  Decompose into: Reproduce with failing test -> Root-cause analysis -> Minimal patch -> Regression verification.
ELSE IF task is a refactoring:
  Decompose into: Characterization tests -> Parallel implementation -> Switch reference -> Deprecate old code.
```

## Recommended Workflow
1. **Analyze**: Read requirements, inspect existing project structure, identify dependencies.
2. **Define Contracts**: Specify Zod schemas, TypeScript interfaces, and API route signatures.
3. **Establish Steps**: Lay out execution steps in chronological order.
4. **Execute Linearly**: Complete Step 1, verify step 1, proceed to Step 2.
5. **Adjust Dynamically**: If an unexpected dependency or constraint is discovered, update the plan immediately.

## Best Practices
- Include concrete file paths in the plan rather than vague concepts (e.g., `src/app/api/auth/route.ts` instead of "create auth endpoint").
- Identify risks early (e.g., third-party API rate limits, hydration mismatches, complex migrations).
- Keep plans visible in the execution trace for agent observability.

## Anti-Patterns
- **Big-Bang Implementation**: Generating 20 files simultaneously without testing compilation between steps.
- **Vague Plans**: "Step 1: Write backend. Step 2: Write frontend. Step 3: Polish." (Unusable for autonomous agents).
- **Plan Drift**: Silently deviating from the stated architectural plan without reconciling the deviation.

## Validation Checklist
- [ ] Does the plan outline specific file targets and contracts before implementation?
- [ ] Are milestones ordered logically to avoid dangling references?
- [ ] Does each phase include an automated validation checkpoint?
- [ ] Can this plan be audited by another agent or human reviewer?

## Related Skills
- `skills/core/decision-making.md`
- `skills/core/context-management.md`
- `skills/testing/testing-strategy.md`

## References
- Agile Task Decomposition Guidelines — https://scrumguides.org/
- Kent Beck's Test-Driven Development by Example

## Last Reviewed
2026-09-15

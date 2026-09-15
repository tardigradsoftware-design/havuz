# Agent Self-Critique & Defect Detection

## Purpose
Provides a rigorous, automated introspective critique process for AI agents to audit their own work, uncover subtle defects, catch edge cases, and eliminate "AI slop" prior to final delivery.

## When To Use
- After completing the implementation phase of any feature, bug fix, or refactor.
- Immediately before declaring a task finished or presenting deliverables to the user.
- When tests fail or build errors occur, to perform root-cause analysis.

## When Not To Use
- In the middle of an atomic code-generation step where interrupting reasoning breaks context flow.

## Core Principles
1. **Assume Imperfection**: Approach completed code with adversarial scrutiny—assume edge cases were missed, types were loosened, and errors were unhandled.
2. **Zero Toleration for Hallucinations**: Verify every imported library, function signature, and CSS class against real project definitions.
3. **User-Centric Empathy**: Evaluate the solution through the eyes of a real user on a slow mobile connection with assistive technology.

## Rules
- **Rule 1 (The 6-Vector Inspection)**: Every critique must systematically audit:
  1. *Type Safety*: Are there any `any`, `@ts-ignore`, or loose type assertions?
  2. *Accessibility*: Are all interactive elements keyboard accessible with visible focus and semantic tags?
  3. *Error Handling*: Does the UI handle empty states, loading skeletons, and server rejection gracefully?
  4. *Performance*: Are there unnecessary re-renders, un-memoized expensive computations, or unoptimized images?
  5. *Responsive Layout*: Does the layout break or overflow horizontally on 375px mobile screens?
  6. *Security*: Are inputs validated with Zod? Are secrets exposed to the client?
- **Rule 2 (No Blind Justification)**: If a potential issue is detected, do not explain away the flaw with prose. Fix the code.

## Decision Criteria
```text
IF any TypeScript error exists:
  REJECT. Resolve type constraint immediately.
IF any interactive element lacks keyboard access:
  REJECT. Replace with semantic button or add keyboard listeners + ARIA.
IF image lacks width/height/alt or priority when above-the-fold:
  REJECT. Add next/image optimizations.
IF database query or mutation lacks input validation:
  REJECT. Enforce Zod schema parsing.
```

## Recommended Workflow
1. Conclude the primary code implementation.
2. Step back and re-read the original user prompt word-for-word.
3. Compare actual file diffs against prompt requirements.
4. Execute the 6-Vector Inspection systematically.
5. If defects are found: apply fixes immediately, re-run tests, and verify.

## Best Practices
- Run automated linters and typecheckers as the first step of self-critique.
- Review your own git diff (`git diff HEAD~1`) to view changes from an external reviewer's perspective.
- Check for forgotten debugging artifacts (`console.log`, temporary hardcoded strings, commented-out dead code).

## Anti-Patterns
- **Self-Congratulation**: Stating "The implementation is now fully robust and complete" without running verification tools.
- **Defensive Rationalization**: Writing "This is just an example, in real production you would add security" instead of writing production-grade code.
- **Ignoring Warnings**: Treating compiler or linter warnings as harmless.

## Validation Checklist
- [ ] Did I review my own git diff?
- [ ] Are all temporary debugging logs and dead comments removed?
- [ ] Are loading, error, and empty states explicitly implemented?
- [ ] Does the implementation strictly meet all acceptance criteria?

## Related Skills
- `skills/core/verification.md`
- `skills/testing/visual-testing.md`
- `skills/security/owasp.md`

## References
- Code Review Best Practices — Google Engineering Practices Guidelines
- Martin Fowler on Refactoring & Code Smells

## Last Reviewed
2026-09-15

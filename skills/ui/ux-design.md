# UX Design & Interaction Architecture

## Purpose
Ensures web applications provide frictionless, intuitive, resilient, and predictable user journeys, minimizing cognitive load and preventing user error.

## When To Use
- When mapping user flows, form workflows, navigation models, and dashboard interactions.
- When designing feedback mechanisms (modals, toasts, inline alerts, empty states, skeletons).
- When architecting complex multi-step processes (e.g., checkout, onboarding, configuration).

## When Not To Use
- For non-interactive, static content pages (though information architecture rules still apply).

## Core Principles
1. **Recognition Over Recall**: Make information, actions, and navigation options visible and self-explanatory rather than forcing the user to remember past screens.
2. **Instant & Predictable Feedback**: Every user interaction must produce immediate feedback (optimistic update, loading spinner, or disabled state).
3. **Forgiving by Design**: Prevent destructive mistakes with undo actions, explicit confirmations, and autosaved drafts.

## Rules
- **Rule 1 (State Completeness)**: Every interactive view must account for 5 fundamental states:
  1. *Empty*: Friendly onboarding guidance and a direct action to create the first entity.
  2. *Loading*: Zero-layout-shift skeletons matching the exact geometry of incoming data.
  3. *Partial/Filtering*: Clear indicator of active filters with a 1-click "Reset all" option.
  4. *Success/Populated*: The primary populated data view.
  5. *Error*: Human-readable explanation with an immediate retry button.
- **Rule 2 (Fitts's Law Target Sizing)**: All interactive touch and click targets must be at least **44x44px** on touch devices and **36px** height on desktop pointer devices.
- **Rule 3 (Destructive Action Guard)**: Any irreversible operation (deleting accounts, revoking API keys, cancelling subscriptions) must require an explicit two-step confirmation dialog or typed confirmation input.
- **Rule 4 (Form Error Proximity)**: Never display form validation errors exclusively in a generic top-of-page banner. Place inline validation messages directly below the offending input field, linked via `aria-describedby`.

## Decision Criteria
```text
IF action is reversible:
  Execute optimistically and show toast with 5-second "Undo" button.
IF action is irreversible and high-impact:
  Open Modal Dialog with explicit confirmation step; DO NOT execute optimistically.
IF page data takes > 200ms to fetch:
  Render skeleton screen; NEVER freeze the UI or show a blank white screen.
```

## Recommended Workflow
1. Map the user's primary mental model and intended goal.
2. Minimize steps: Eliminate redundant form fields and optional questions from the critical path.
3. Wireframe the standard golden path and the error edge cases.
4. Implement interactive controls with clear focus, hover, active, and disabled states.
5. Validate user flow friction with user persona walk-throughs.

## Best Practices
- Retain form values in local storage or URL query state to prevent loss on accidental refresh.
- Disable submit buttons during form submission to prevent duplicate network requests (`isSubmitting ? disabled : enabled`).
- Use contextual tooltips for technical terminology or complex data metrics.

## Anti-Patterns
- **Mystery Meat Navigation**: Icons without labels or tooltips, forcing users to guess function.
- **Infinite Spinners**: Full-screen blocking spinners that give zero context on what is loading.
- **Sudden Layout Pops**: Content popping in abruptly after fetch, jumping the user's scroll position.

## Validation Checklist
- [ ] Are empty, loading, error, and success states implemented for every data view?
- [ ] Are all click targets at least 44x44px for touch interfaces?
- [ ] Are destructive actions protected by modal confirmation?
- [ ] Do all forms prevent double submission on rapid clicks?

## Related Skills
- `skills/ui/ui-design.md`
- `skills/ui/accessibility.md`
- `skills/saas/onboarding.md`

## References
- Nielsen Norman Group 10 Usability Heuristics — https://www.nngroup.com/articles/ten-usability-heuristics/
- Laws of UX — Jon Yablonski — https://lawsofux.com/

## Last Reviewed
2026-09-15

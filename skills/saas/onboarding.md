# SaaS Onboarding, Time-to-Value (TTV) & Empty States

## Purpose
Maximizes user activation and reduces Time-to-Value (TTV) during the critical first 5 minutes of a user's experience with a SaaS platform through guided onboarding and actionable empty states.

## When To Use
- When engineering the post-signup experience, initial workspace setup, and onboarding wizards.
- When designing first-time user experiences and empty collection views.

## When Not To Use
- For established enterprise accounts where data is already populated and users have completed onboarding.

## Core Principles
1. **Minimize Time to Aha! Moment**: Guide the user to the core value proposition of the product within the first 60 seconds of signing up.
2. **Actionable Empty States**: An empty screen must never be a dead end. Every empty state must educate the user and provide a 1-click action to create their first entity.
3. **Progressive Disclosure**: Do not overwhelm new users with 50 settings and configuration options upfront. Reveal advanced capabilities progressively as they master basic workflows.

## Rules
- **Rule 1 (The 3-Step Setup Wizard Standard)**:
  Post-signup onboarding must not exceed 3 concise, low-friction steps:
  1. *Workspace Name & URL Slug* (pre-filled with company or user name).
  2. *Primary Use Case / Role* (helps tailor dashboard defaults).
  3. *Core Action* (e.g. "Create your first project" or "Connect GitHub repository").
  - Always provide a subtle "Skip for now" link so experienced users are never trapped.
- **Rule 2 (Actionable Empty State Architecture)**:
  Every empty collection view (empty project list, empty invoice table, empty team list) must render:
  - Clear icon or visual metaphor.
  - Conversational headline: "No projects created yet".
  - One-sentence benefit description: "Projects help your team organize tasks, track milestones, and share documentation."
  - Primary Action Button: "Create First Project" (triggers modal or creation form).
- **Rule 3 (Interactive Setup Checklist / Getting Started Widget)**:
  Display an interactive checklist on the dashboard until all initial setup tasks are complete:
  - 4–5 achievable milestones (e.g. [✓] Create account, [ ] Add project, [ ] Invite a teammate, [ ] Connect domain).
  - Progress bar (e.g. "50% completed").
  - Dismissible with a 1-click "Hide checklist" option.
- **Rule 4 (Demo / Seed Data Option)**:
  For complex analytics or workflow platforms, provide a 1-click "Load Sample Data" button so users can explore interactive charts and tables before importing real business data.

## Decision Criteria
```text
IF user account is new (< 7 days old) and has 0 projects:
  Display Getting Started Checklist widget prominently on dashboard.
IF user clicks "Skip" on onboarding wizard:
  Set onboardingCompleted: true in user profile and navigate directly to dashboard.
IF all checklist tasks are completed:
  Trigger congratulatory micro-animation (confetti / toast) and collapse checklist.
```

## Recommended Workflow
1. Identify the single "Aha! Moment" that proves value to the user.
2. Streamline signup form: eliminate all non-essential fields (ask only for email and password/OAuth).
3. Build lightweight onboarding modal or route (`/onboarding`).
4. Design actionable empty states for all main resource views.
5. Implement setup checklist tracking in user/organization database record.

## Best Practices
- Pre-fill inputs with intelligent defaults (e.g., derive workspace name from user's email domain).
- Celebrate early wins with subtle micro-interactions.
- Provide contextual tooltips or inline guidance on complex forms.

## Anti-Patterns
- **The Empty White Screen**: Dumping a brand new user into a blank dashboard with no instructions or buttons, causing immediate churn.
- **The 12-Step Interrogation**: Forcing users to fill out 12 survey questions about their company before allowing them to see the application.
- **The Annoying Tour Walkthrough**: Covering the screen with 15 blocking tooltips ("Click next to see what this button does").

## Validation Checklist
- [ ] Post-signup onboarding completes in under 3 steps with a "Skip" option.
- [ ] Every empty view features an explanatory description and primary action button.
- [ ] Setup checklist accurately tracks completed milestones.
- [ ] Time-to-Value (TTV) is under 2 minutes for first project creation.

## Related Skills
- `skills/saas/dashboard-ux.md`
- `skills/ui/ux-design.md`
- `skills/ecommerce/conversion.md`

## References
- ProductLed Onboarding Architecture — Wes Bush
- UserOnboard Teardowns & Heuristics — Samuel Hulick

## Last Reviewed
2026-09-15

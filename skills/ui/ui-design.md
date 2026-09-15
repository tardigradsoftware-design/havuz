# Modern UI Design & Component Craft

## Purpose
Governs the creation of visually refined, balanced, high-converting, and modern user interfaces using systematic design tokens, clean composition, and deliberate restraint.

## When To Use
- When designing or implementing page layouts, dashboards, cards, headers, navigation bars, and marketing sections.
- When selecting color palettes, border radiuses, elevation levels, and spatial relationships.
- When refining raw wireframes into production-grade user interfaces.

## When Not To Use
- When configuring pure backend services, API contracts, or CLI tools lacking a visual presentation layer.

## Core Principles
1. **Restraint Over Clutter**: Elegance stems from generous whitespace, disciplined color palettes, and typographic precision—not superficial gradients, excessive borders, or random decorations.
2. **Design Tokens First**: Never use arbitrary one-off CSS values (e.g., `margin: 17px; background: #e38a21;`). All values must map to a unified design token system.
3. **Purposeful Contrast**: Every visual distinction (color tint, weight, border) must signify structural or functional meaning to the user.

## Rules
- **Rule 1 (The Spacing Grid)**: Use the strict 4px/8px baseline grid exclusively:
  - 4px (`p-1` / `gap-1`): Micro-spacing inside tags, badges, and icon buttons.
  - 8px (`p-2` / `gap-2`): Compact button padding, dropdown item spacing.
  - 16px (`p-4` / `gap-4`): Card interior padding, standard input element height spacing.
  - 24px (`p-6` / `gap-6`): Card separation, section internal gutters.
  - 32px / 48px / 64px (`py-8` / `py-12` / `py-16`): Major layout and section spacing.
- **Rule 2 (Color System Limits)**: Maintain a disciplined palette:
  - Base: 1 background color, 1 surface color, 1 subtle elevated surface color.
  - Text: 1 primary text (high contrast), 1 secondary text (muted contrast), 1 tertiary text (placeholder/disabled).
  - Accent: Exactly 1 primary brand action color. 1 destructive/error color. 1 success color. 1 warning color.
  - Avoid using multiple competing primary colors on the same viewport screen.
- **Rule 3 (Border & Shadow Hierarchy)**:
  - Prefer subtle borders (`border border-neutral-200 dark:border-neutral-800`) over heavy drop shadows.
  - Elevation shadows must be soft, multi-layered, and natural (e.g., `shadow-sm`, `shadow-md` with low opacity `rgba(0,0,0,0.05)`).
- **Rule 4 (Component Reusability)**: Every UI element must be a composable, reusable atomic component built with variant control (e.g., `cva` - `class-variance-authority`).

## Decision Criteria
```text
IF element is primary action on screen:
  Use solid primary brand background with high-contrast text (e.g., bg-primary text-primary-foreground).
ELSE IF element is secondary action:
  Use outline or ghost style (e.g., variant="outline" or variant="ghost").
ELSE IF element is destructive:
  Use variant="destructive" with confirmation dialog before execution.
```

## Recommended Workflow
1. Establish base theme tokens in `globals.css` or Tailwind configuration (HSL/OKLCH color variables).
2. Scaffold major layout containers with responsive max-widths (`max-w-7xl mx-auto px-4 sm:px-6 lg:px-8`).
3. Compose view using atomic primitives (`Button`, `Card`, `Badge`, `Input`) from `shadcn/ui` or equivalent.
4. Verify visual balance, alignment, and whitespace rhythm.
5. Review with `checklists/ui-review.md`.

## Best Practices
- Keep radius consistent: If cards use `rounded-xl`, nested interactive elements should use `rounded-lg` or `rounded-md`.
- Ensure dark mode support from day one using CSS variables, not hardcoded hex values.
- Align icons with text baseline using `inline-flex items-center gap-2`.

## Anti-Patterns
- **Rainbow UI**: Using 6 different vibrant colors across buttons and badges, confusing visual hierarchy.
- **Arbitrary Magic Numbers**: `top-[23px]`, `w-[317px]`, `p-[11px]`.
- **Text Low-Contrast Abuse**: Using light gray text `#a0a0a0` on white backgrounds, violating WCAG standards.

## Validation Checklist
- [ ] Are all margins and paddings aligned to 4px/8px multiples?
- [ ] Does the screen feature a single, unambiguous primary Call-to-Action?
- [ ] Is dark mode supported seamlessly without unreadable low-contrast surfaces?
- [ ] Are all components derived from standard design system primitives?

## Related Skills
- `skills/ui/visual-hierarchy.md`
- `skills/ui/typography.md`
- `skills/ui/accessibility.md`

## References
- Refactoring UI — Adam Wathan & Steve Schoger
- Radix UI Design Tokens & Theme Specification — https://www.radix-ui.com/themes

## Last Reviewed
2026-09-15

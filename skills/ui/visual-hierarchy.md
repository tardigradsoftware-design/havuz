# Visual Hierarchy & Layout Composition

## Purpose
Establishes clear focal points, reading pathways, and spatial ordering so users instantly comprehend content importance, relationships, and next steps.

## When To Use
- When laying out page content, marketing landing pages, data dashboards, and complex cards.
- When structuring document content, headers, hero banners, and feature grids.

## When Not To Use
- For raw data tables or unstructured data streaming logs.

## Core Principles
1. **Size Signifies Importance**: The most important element on any viewport must be visibly distinct in scale, contrast, or spatial dominance.
2. **Proximity Indicates Relationship (Gestalt)**: Elements that belong together must be grouped with tighter spacing than unrelated surrounding elements.
3. **Scanning Flow (Z-Pattern & F-Pattern)**: Design layouts to support natural human scanning habits:
   - Z-Pattern for visual, marketing, and landing pages.
   - F-Pattern for text-heavy, data-dense dashboards and search results.

## Rules
- **Rule 1 (Single Hero Focal Point)**: Never place two competing visual focal points above the fold. There must be exactly one primary headline (`h1`) and one primary action button per viewport screen.
- **Rule 2 (The Scale Multiplier)**: Headings must follow a distinct, non-ambiguous typographical scale ratio (at least 1.25x - Major Third, or 1.33x - Perfect Fourth):
  - `h1`: 36px–48px (bold/extrabold)
  - `h2`: 24px–30px (semibold/bold)
  - `h3`: 18px–20px (semibold)
  - `body`: 14px–16px (regular)
  - `caption`: 12px (regular/medium)
- **Rule 3 (Spatial Grouping Hierarchy)**:
  - Inside a single item (e.g. icon + label): `gap-1` or `gap-2` (4px–8px).
  - Between related form fields: `gap-4` (16px).
  - Between distinct sections inside a card: `gap-6` (24px).
  - Between distinct page sections: `gap-12` to `gap-20` (48px–80px).
- **Rule 4 (Contrast Stratification)**:
  - 1st Tier (Highest): Primary headings, primary buttons, critical alerts.
  - 2nd Tier: Body copy, secondary buttons, section labels.
  - 3rd Tier (Lowest): Helper text, timestamps, borders, subtle metadata.

## Decision Criteria
```text
IF user needs to scan multiple features quickly:
  Use a 3-column grid (grid-cols-1 md:grid-cols-3 gap-8) with consistent icon, title, and body hierarchy.
IF page is a high-value landing page:
  Stack vertically with clear rhythm: Hero -> Social Proof -> Value Prop -> Deep Features -> Pricing -> CTA.
IF user must make a decision between options:
  Visually elevate the recommended/popular option with a badge, border highlight, and scale accent.
```

## Recommended Workflow
1. Identify the single most important action or message on the screen.
2. Assign visual weight (size, weight, color contrast) proportionally to importance.
3. Apply Gestalt proximity: group related fields with cards or tight containers.
4. Test scanning readability by squinting or applying blur: does the primary action remain obvious?
5. Validate hierarchy across desktop and mobile screens.

## Best Practices
- Use uppercase and letter-spacing (`tracking-wider text-xs font-semibold uppercase text-muted-foreground`) exclusively for small section eyebrow labels.
- Keep card heights aligned using flex column layout (`flex flex-col justify-between`).
- Use whitespace as a separator before defaulting to horizontal dividing lines (`<hr />`).

## Anti-Patterns
- **Flatland Syndrome**: Everything is 16px font and medium gray; user cannot find what matters.
- **Compete-For-Attention**: Blinking badges, 4 different colored buttons, and 3 banners all shouting at the user.
- **Inverted Proximity**: Spacing between a heading and its subtext being larger than the spacing to the previous section.

## Validation Checklist
- [ ] Is there exactly one primary `h1` per page?
- [ ] Can a user identify the primary action within 3 seconds of looking at the screen?
- [ ] Does grouping clearly distinguish related vs. unrelated data elements?
- [ ] Are headings visibly differentiated from body copy by at least 1.25x scale?

## Related Skills
- `skills/ui/ui-design.md`
- `skills/ui/typography.md`
- `skills/ecommerce/conversion.md`

## References
- Gestalt Principles of Perceptual Organization
- The Design of Everyday Things — Don Norman

## Last Reviewed
2026-09-15

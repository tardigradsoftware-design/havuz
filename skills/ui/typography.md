# Modern Typography Systems & Legibility

## Purpose
Establishes a robust, legible, and harmonious typographic hierarchy that enhances reading comprehension, brand identity, and accessibility.

## When To Use
- When configuring web font stacks, typographic scales, line heights, letter spacing, and line lengths.
- When styling prose content, blog articles, documentation, headers, and UI microcopy.

## When Not To Use
- For pure canvas-rendered WebGL games or raw binary data representations.

## Core Principles
1. **Legibility First**: Typography exists to be read. Font choices, weights, and contrasts must facilitate effortless scanning and extended reading.
2. **Proportional Leading (Line Height)**: As font size increases, relative line height (`line-height`) must decrease:
   - Body copy (16px): `line-height: 1.5–1.6` (e.g., `leading-relaxed`).
   - Large headings (36px–64px): `line-height: 1.1–1.2` (e.g., `leading-tight` or `leading-none`).
3. **The 65-Character Measure**: Ideal reading line length for body copy is **45 to 75 characters** (optimal ~65 characters). Never allow body text to stretch across a 1920px screen without a `max-w-prose` constraint.

## Rules
- **Rule 1 (The Modern Font Stack)**: Standardize on clean, high-performance font stacks:
  - Sans-Serif: `Inter`, `Geist Sans`, or system font fallback:
    `font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;`
  - Monospace (Code/Data): `Geist Mono`, `JetBrains Mono`, or `ui-monospace`.
  - Always apply `font-display: swap` in `@font-face` or `next/font` configuration to prevent FOIT (Flash of Invisible Text).
- **Rule 2 (Strict Typographic Scale)**:
  - Hero display: `text-5xl md:text-6xl font-extrabold tracking-tight`
  - Page Heading (`h1`): `text-3xl md:text-4xl font-bold tracking-tight`
  - Section Heading (`h2`): `text-2xl md:text-3xl font-semibold tracking-tight`
  - Subsection Heading (`h3`): `text-xl font-semibold`
  - Card Title (`h4`): `text-lg font-medium`
  - Body Text: `text-base text-muted-foreground leading-relaxed`
  - Microcopy / Metadata: `text-xs md:text-sm text-muted-foreground`
- **Rule 3 (Tracking & Kerning)**:
  - Headings (> 24px): Apply negative tracking (`tracking-tight` or `letter-spacing: -0.02em`) to tighten optical space.
  - Eyebrows & Badges (< 12px uppercase): Apply positive tracking (`tracking-wider` or `letter-spacing: 0.05em`).
- **Rule 4 (Contrast Compliance)**:
  - Body text must meet **WCAG 2.2 AA** minimum contrast ratio of **4.5:1** against background.
  - Large text (>= 24px regular or >= 18.6px bold) must meet at least **3:1**.

## Decision Criteria
```text
IF element is long-form prose (blog post, documentation):
  Apply Tailwind Typography plugin class `prose dark:prose-invert max-w-prose`.
IF element is numeric dashboard metric:
  Use monospace numerals `tabular-nums font-semibold` to prevent layout jitter during number updates.
IF text is an interactive button label:
  Use medium/semibold weight (`font-medium`) with `text-sm` for crisp geometric rendering.
```

## Recommended Workflow
1. Load fonts via `next/font/google` or `next/font/local` for zero-layout-shift font preloading.
2. Bind font variables to Tailwind (`font-sans: ['var(--font-sans)', ...]`).
3. Set base body typography in `app/layout.tsx`.
4. Apply semantic heading tags (`h1` through `h6`) strictly reflecting document outline.
5. Verify contrast ratios using Chrome DevTools or Lighthouse.

## Best Practices
- Never use all-caps for long sentences or paragraphs; reserve uppercase strictly for badges, abbreviations, or micro-labels.
- Ensure tabular numbers (`tabular-nums`) are applied to countdown clocks, stock tickers, and tabular prices.
- Avoid italicizing large blocks of text; use font weight or color shift for emphasis.

## Anti-Patterns
- **The Never-Ending Line**: A 16px paragraph stretching full width across an ultra-wide monitor, causing extreme eye strain.
- **Too Many Font Families**: Loading 4 different decorative fonts on one website, destroying performance and visual cohesion.
- **Suffocating Headings**: Setting `line-height: 1.6` on a 48px heading, creating awkward massive gaps between wrapped words.

## Validation Checklist
- [ ] Are fonts loaded with `font-display: swap` or `next/font` zero-shift loaders?
- [ ] Does body text contrast exceed 4.5:1 against the background?
- [ ] Is long-form text constrained to `max-w-prose` (65-75 characters)?
- [ ] Do numeric metrics use `tabular-nums`?

## Related Skills
- `skills/ui/visual-hierarchy.md`
- `skills/ui/accessibility.md`
- `skills/performance/core-web-vitals.md`

## References
- The Elements of Typographic Style Applied to the Web — Richard Rutter
- Butterick’s Practical Typography — Matthew Butterick

## Last Reviewed
2026-09-15

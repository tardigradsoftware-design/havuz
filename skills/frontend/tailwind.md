# Modern Tailwind CSS & Design Token Architecture

## Purpose
Enforces scalable, maintainable, zero-runtime utility-first CSS using modern Tailwind CSS (v4 / v3.4+), CSS variable design tokens, and composable class-variance systems.

## When To Use
- In all styling, layout, typography, responsive styling, and theme implementations.
- When creating reusable design system primitives (`Button`, `Card`, `Badge`).

## When Not To Use
- Inside third-party canvas WebGL renderers or isolated SVG vector path calculations.

## Core Principles
1. **Zero-Runtime CSS**: Styling must be computed at build/compile time. Avoid CSS-in-JS libraries that inject runtime `<style>` tags.
2. **CSS Variables for Dynamic Theming**: Define color tokens as CSS variables (`--background`, `--foreground`, `--primary`) to make light/dark mode and multi-branding effortless.
3. **Class Variance Authority (CVA)**: Manage multi-variant components cleanly with `cva` and `tailwind-merge`.

## Rules
- **Rule 1 (The Utility Merging Standard)**:
  Always combine dynamic classes using `cn()` (`clsx` + `tailwind-merge`):
  ```typescript
  import { clsx, type ClassValue } from 'clsx';
  import { twMerge } from 'tailwind-merge';

  export function cn(...inputs: ClassValue[]) {
    return twMerge(clsx(inputs));
  }
  ```
  This guarantees that passing `className="p-4"` to a component with default `p-2` cleanly resolves to `p-4` without CSS precedence conflicts.
- **Rule 2 (Variant Control via CVA)**:
  Structure multi-variant components with `class-variance-authority`:
  ```typescript
  import { cva, type VariantProps } from 'class-variance-authority';

  export const buttonVariants = cva(
    'inline-flex items-center justify-center rounded-md font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50',
    {
      variants: {
        variant: {
          default: 'bg-primary text-primary-foreground hover:bg-primary/90',
          destructive: 'bg-destructive text-destructive-foreground hover:bg-destructive/90',
          outline: 'border border-input bg-background hover:bg-accent hover:text-accent-foreground',
          ghost: 'hover:bg-accent hover:text-accent-foreground',
        },
        size: {
          sm: 'h-8 px-3 text-xs',
          md: 'h-10 px-4 py-2 text-sm',
          lg: 'h-12 px-8 text-base',
        },
      },
      defaultVariants: {
        variant: 'default',
        size: 'md',
      },
    }
  );
  ```
- **Rule 3 (No Arbitrary Magic Values)**:
  Never write one-off arbitrary values like `top-[13px]`, `w-[327px]`, or `bg-[#ff0033]`. Extend theme tokens in Tailwind configuration or CSS variables.
- **Rule 4 (Dark Mode Standardization)**:
  Use CSS class-based or media-query dark mode (`class` strategy) with semantic color tokens:
  `bg-background text-foreground border-border`. Never hardcode `bg-white dark:bg-black` manually on every single div.

## Decision Criteria
```text
IF element has multiple stylistic states (size, variant, intent):
  Use CVA (class-variance-authority).
IF element receives custom consumer className:
  Merge using `cn(variants({ variant, size }), className)`.
IF creating a color:
  Map to semantic token (`bg-primary`, `bg-muted`, `bg-card`); NEVER use raw hex.
```

## Recommended Workflow
1. Configure design tokens as CSS variables in `globals.css` (`:root` and `.dark`).
2. Map variables to Tailwind theme in CSS (`@theme` in Tailwind v4) or `tailwind.config.ts`.
3. Build atomic UI primitives using `cva` and `cn`.
4. Compose application layouts cleanly using utility classes.
5. Audit generated CSS bundle size to ensure proper tree-shaking.

## Best Practices
- Group classes logically: Layout (`flex`, `grid`) -> Spacing (`p-4`, `m-2`) -> Sizing (`w-full`, `h-10`) -> Typography (`text-sm`, `font-medium`) -> Colors (`bg-card`, `text-card-foreground`) -> States (`hover:`, `focus-visible:`).
- Use container queries (`@container`) for modular cards that change layout based on parent container width rather than full viewport width.
- Use `space-y-*` or flex `gap-*` consistently; prefer `gap-*` in modern flex/grid layouts.

## Anti-Patterns
- **String Interpolation Disasters**: `className={isLarge ? 'text-lg ' + myClass : 'text-sm'}` (Fails on Tailwind class conflicts; use `cn()`).
- **Dynamic Tailwind Class Construction**: `className={`text-${color}-500`}` (Tailwind purge cannot parse dynamic template strings at build time; classes will fail to compile).
- **Inline Style Resuscitation**: Falling back to `style={{ padding: '12px' }}` when utility classes already exist.

## Validation Checklist
- [ ] Are all dynamic class combinations wrapped in `cn()`?
- [ ] Are color tokens semantic (`primary`, `muted`, `accent`) rather than hardcoded hex?
- [ ] Does dark mode function properly across all components without hardcoded light values?
- [ ] Zero arbitrary brackets used for standard spacing and color intervals.

## Related Skills
- `skills/ui/ui-design.md`
- `skills/frontend/component-architecture.md`
- `references/design-systems/shadcn-radix.md`

## References
- Tailwind CSS Official Documentation — https://tailwindcss.com/docs
- CVA (Class Variance Authority) Documentation — https://cva.style/docs

## Last Reviewed
2026-09-15

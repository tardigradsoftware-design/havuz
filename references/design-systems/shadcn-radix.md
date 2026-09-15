# shadcn/ui & Radix UI Integration Reference

## 1. Philosophy: Copy-Paste Architecture
- **shadcn/ui is NOT an npm component library**. It is a reusable component distribution system where the code lives directly inside your repository (`src/components/ui/`).
- Developers own every single line of code, enabling complete customization of markup, Tailwind classes, and accessibility variants.

---

## 2. Core Building Blocks

1. **Radix Primitives**: Headless, unstyled UI primitives managing complex WAI-ARIA states, keyboard focus, and screen reader announcements (`@radix-ui/react-*`).
2. **Tailwind CSS**: Utility classes styling the headless primitives.
3. **Class Variance Authority (`cva`)**: Declarative variant manager mapping props (`variant="outline"`, `size="lg"`) to Tailwind class sets.
4. **Tailwind Merge (`tailwind-merge`)**: Resolves class collisions when consumers override default styles.

---

## 3. Directory Layout

```text
src/
└── components/
    └── ui/
        ├── button.tsx
        ├── dialog.tsx
        ├── dropdown-menu.tsx
        ├── input.tsx
        ├── table.tsx
        ├── sheet.tsx
        └── badge.tsx
```

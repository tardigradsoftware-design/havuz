# Modern UI Design Patterns & Spatial Standards

## 1. 4px / 8px Spatial Scale

| Class | Pixel Value | Typical Application |
| :--- | :--- | :--- |
| `gap-1` / `p-1` | 4px | Internal tag/badge margins, button icon spacing |
| `gap-2` / `p-2` | 8px | Button internal padding, dropdown item padding |
| `gap-3` / `p-3` | 12px | Compact card gutters, list item separation |
| `gap-4` / `p-4` | 16px | Form input heights, standard card padding |
| `gap-6` / `p-6` | 24px | Card grids, modal interior spacing |
| `gap-8` / `p-8` | 32px | Section gutters, page header margins |
| `py-12` / `py-16`| 48px / 64px | Landing page section padding |
| `py-20` / `py-24`| 80px / 96px | Hero banner vertical padding |

---

## 2. Elevation & Shadow Token Hierarchy

```css
/* Subtle 1-pixel borders preferred over heavy drop shadows */
.card-border {
  border: 1px solid var(--border);
}

/* Natural ambient drop shadow */
.card-shadow {
  box-shadow: 0 1px 3px 0 rgb(0 0 0 / 0.05), 0 1px 2px -1px rgb(0 0 0 / 0.05);
}

/* Elevated hover state */
.card-shadow-hover:hover {
  box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.07), 0 2px 4px -2px rgb(0 0 0 / 0.05);
}
```

---

## 3. Container Width Constraints

- Mobile viewport: `w-full px-4` (375px–640px)
- Tablet container: `max-w-3xl mx-auto px-6` (768px)
- Standard desktop layout: `max-w-7xl mx-auto px-8` (1280px)
- Prose / article container: `max-w-prose mx-auto` (~65ch width for optimal reading)

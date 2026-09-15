# WCAG 2.2 AA Accessibility Quick Reference

## 1. Contrast Requirements

| Text Type | Font Size / Weight | Minimum Contrast Ratio |
| :--- | :--- | :--- |
| **Normal Body Text** | < 24px regular or < 18.6px bold | **4.5:1** against background |
| **Large Headings** | >= 24px regular or >= 18.6px bold | **3.0:1** against background |
| **UI Components & Icons**| Interactive borders, status icons | **3.0:1** against adjacent colors |

---

## 2. Critical ARIA Patterns

| Interaction | Required ARIA Attributes | Keyboard Requirements |
| :--- | :--- | :--- |
| **Dialog / Modal** | `role="dialog"`, `aria-modal="true"`, `aria-labelledby="dialog-title"` | Traps focus; closes on `Escape`; restores trigger focus on close |
| **Dropdown Menu** | `role="menu"`, `aria-haspopup="menu"`, `aria-expanded="boolean"` | Arrows to navigate items; `Enter`/`Space` to select; `Escape` to close |
| **Accordion** | `role="region"`, `aria-controls="panel-id"`, `aria-expanded="boolean"` | `Enter`/`Space` toggles; Tab cycles between headers |
| **Alert / Toast** | `role="status"` or `role="alert"`, `aria-live="polite"` | Announced automatically by screen reader without stealing focus |

---

## 3. Touch Target Standards (WCAG 2.2 Criterion 2.5.8)
- Minimum target size: **24x24 CSS pixels** (WCAG AA).
- Ergonomic standard: **44x44 CSS pixels** on mobile touchscreens.

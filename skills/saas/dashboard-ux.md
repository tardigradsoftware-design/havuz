# High-Density Dashboard UX, Data Tables & Command Palettes

## Purpose
Governs the design and implementation of productivity-focused, high-density SaaS dashboards, interactive data tables, metrics cards, and keyboard-driven command navigation.

## When To Use
- When engineering SaaS application consoles, analytical dashboards, reporting views, and data management tables.
- When creating command menus (`cmd+k`), search filters, and bulk data operations.

## When Not To Use
- For high-converting consumer marketing landing pages or simple static content sites.

## Core Principles
1. **Information Density with Breathing Room**: Maximize data throughput without sacrificing visual clarity. Use compact typography, tight tabular padding, and muted dividing lines.
2. **Keyboard First (Power User Ergonomics)**: Power users must be able to search, navigate, and execute frequent actions without lifting their hands from the keyboard (`Cmd+K`).
3. **Data Scannability**: Highlight anomalous metrics (negative trends in red, positive in green), summarize complex tables with sparklines or metric cards, and support instantaneous column sorting.

## Rules
- **Rule 1 (Top-Level Metrics Summary Cards)**:
  Every analytics or overview view must feature 3–4 key performance indicator (KPI) cards above the fold:
  ```tsx
  <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
    <Card>
      <CardHeader className="flex flex-row items-center justify-between pb-2">
        <CardTitle className="text-sm font-medium text-muted-foreground">Total Revenue</CardTitle>
        <DollarSign className="h-4 w-4 text-muted-foreground" />
      </CardHeader>
      <CardContent>
        <div className="text-2xl font-bold tabular-nums">$45,231.89</div>
        <p className="text-xs text-emerald-500 font-medium flex items-center gap-1 mt-1">
          <ArrowUpRight className="h-3 w-3" /> +20.1% from last month
        </p>
      </CardContent>
    </Card>
  </div>
  ```
- **Rule 2 (Interactive Data Table Standard)**:
  Data tables must support:
  - Column sorting with clear visual sort indicators (↑ / ↓).
  - Search filtering with URL query state persistence.
  - Column visibility toggling.
  - Multi-row selection checkboxes with bulk action bar (e.g., "Delete 12 selected").
  - Pagination with customizable page sizes (10, 25, 50, 100).
- **Rule 3 (Command Palette Standard - Cmd+K)**:
  Implement a global keyboard-driven command menu using `cmdk` / `shadcn/ui`:
  - Shortcut: `⌘+K` (macOS) / `Ctrl+K` (Windows).
  - Capabilities: Quick jump to any page/project, quick actions ("Create new invoice", "Invite member"), and global search.
- **Rule 4 (Responsive Tabular Adaptation)**:
  Data tables with > 5 columns must never break mobile screens:
  - Desktop (`lg:`): Full interactive table.
  - Mobile (`< lg`): Responsive card stack or horizontal scroll wrapper with sticky header and sticky first column.

## Decision Criteria
```text
IF table contains > 1,000 rows:
  Implement server-side pagination, sorting, and filtering via URL query params.
IF user presses ⌘K:
  Open command dialog, focus search input, and trap keyboard navigation.
IF metric card displays currency or metrics:
  Apply `tabular-nums` class to prevent font width jitter during real-time value changes.
```

## Recommended Workflow
1. Identify primary metrics and operational tables needed by user persona.
2. Assemble top KPI cards with trend indicators.
3. Implement data table using TanStack Table (`@tanstack/react-table`) and shadcn primitives.
4. Wire global command palette (`CommandDialog`).
5. Verify keyboard navigation across table rows using arrow keys and space/enter for selection.

## Best Practices
- Display empty states with an illustration, helpful description, and a primary action button ("Create your first project").
- Keep table cell padding compact (`py-3 px-4`).
- Provide instant CSV export for enterprise tabular data.

## Anti-Patterns
- **Unformatted Data Dumps**: Printing raw ISO timestamps (`2026-09-15T10:14:22.124Z`) in table cells instead of human-friendly dates (`Sep 15, 2026`).
- **Missing Loading States**: Freezing the entire table for 2 seconds while sorting instead of showing subtle row skeletons or progress bar.
- **Unusable Mobile Tables**: Allowing table headers to wrap into 5 lines of text on phone screens.

## Validation Checklist
- [ ] Command menu opens on `Cmd+K` and navigates smoothly.
- [ ] Data tables support sorting, filtering, and pagination.
- [ ] Numbers use `tabular-nums` for alignment.
- [ ] Tables degrade gracefully on mobile screens.

## Related Skills
- `skills/saas/saas-architecture.md`
- `skills/frontend/state-management.md`
- `skills/ui/visual-hierarchy.md`

## References
- TanStack Table Documentation — https://tanstack.com/table/v8
- CMDK Command Menu Component — https://cmdk.paco.me/

## Last Reviewed
2026-09-15

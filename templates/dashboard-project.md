# Template: High-Density Analytics & Data Workspace Blueprint

> **Archetype**: Enterprise Data Console & Operations Workspace  
> **Key Focus**: Tabular data manipulation, live metric visualization, real-time filtering, and zero input latency.

---

## 1. Executive Summary & Specification
- **Target Audience**: Financial analysts, data engineers, operations managers.
- **Conversion Goal**: Rapid insight discovery, report generation, error detection.
- **Key Metrics**: INP < 100ms, sub-second table filtering across 50,000 records.

---

## 2. Recommended File Hierarchy

```text
src/
├── app/
│   ├── (console)/
│   │   ├── layout.tsx            # Collapsible compact sidebar, quick search, status bar
│   │   ├── analytics/page.tsx    # Live charts, timeframe selector, export trigger
│   │   ├── records/page.tsx      # Virtualized / paginated data table
│   │   └── audit-log/page.tsx    # Chronological system event log
├── components/
│   ├── charts/                   # Recharts / Tremor modular chart components (dynamic import)
│   ├── data-table/               # TanStack Table column definitions, filter inputs, pagination
│   └── command/                  # Global Cmd+K quick navigation palette
└── lib/
    ├── query-client.ts           # TanStack Query instance with optimistic caching
    └── table-filters.ts          # URL query parsing with nuqs
```

---

## 3. Mandatory Skill Set to Load

1. `skills/saas/dashboard-ux.md`
2. `skills/frontend/component-architecture.md`
3. `skills/frontend/state-management.md`
4. `skills/performance/javascript-performance.md`
5. `skills/ui/visual-hierarchy.md`
6. `skills/ui/accessibility.md`
7. `skills/testing/visual-testing.md`

---

## 4. Execution Pipeline

1. **Step 1**: Build collapsible sidebar with memory persistence in cookies.
2. **Step 2**: Assemble KPI metric cards with `tabular-nums` formatting and sparklines.
3. **Step 3**: Implement dynamic import for heavy charting libraries via `next/dynamic`.
4. **Step 4**: Connect data tables to type-safe URL query parameters using `nuqs`.
5. **Step 5**: Add `Cmd+K` command menu for keyboard navigation.
6. **Step 6**: Execute `checklists/ui-review.md` and `checklists/responsive-review.md`.

# Google Search Essentials & Core Web Vitals Reference

## 1. Technical Crawling Principles

1. **HTTP Status Codes**:
   - `200`: Success (indexable).
   - `301`: Permanent redirect (transfers search equity).
   - `404` / `410`: Not found / Gone (removes from index).
   - **Never return 200 for missing pages (Soft 404).**
2. **Robots.txt Location**: Must be served at domain root `/robots.txt`.
3. **Sitemap Protocol**: Must be served as valid XML at `/sitemap.xml`.

---

## 2. Core Web Vitals Thresholds (Google 2026 Standards)

```text
┌─────────────────────────────────┬───────────┬──────────────┬────────────┐
│ Metric                          │ Good      │ Needs Work   │ Poor       │
├─────────────────────────────────┼───────────┼──────────────┼────────────┤
│ LCP (Largest Contentful Paint)  │ <= 2.5s   │ 2.5s - 4.0s  │ > 4.0s     │
│ INP (Interaction to Next Paint) │ <= 200ms  │ 200ms - 500ms│ > 500ms    │
│ CLS (Cumulative Layout Shift)   │ <= 0.1    │ 0.1 - 0.25   │ > 0.25     │
└─────────────────────────────────┴───────────┴──────────────┴────────────┘
```

---

## 3. Metadata Formula

- **Title Tag**: `[Primary Keyword] - [Context] | [Brand]` (50–60 characters)
- **Meta Description**: Active benefit statement with primary keyword (140–160 characters)
- **Canonical**: `<link rel="canonical" href="https://example.com/clean-path" />`

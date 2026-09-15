# E-Commerce SEO & Faceted Navigation Architecture

## Purpose
Maximizes search organic revenue for e-commerce catalogs by optimizing product listing pages (PLP), product detail pages (PDP), category taxonomy, and faceted navigation crawl budgets.

## When To Use
- In all online retail stores, digital product marketplaces, and catalog browsing experiences.
- When designing multi-filter systems (brand, size, color, price) and pagination.

## When Not To Use
- For single-product landing pages or non-catalog content sites.

## Core Principles
1. **Faceted Navigation Crawl Budget Control**: Millions of filter combinations can destroy search crawl budgets; index only high-value search facets and canonicalize the rest.
2. **Canonical Category Anchoring**: Sub-filtered views must canonicalize back to the primary canonical category page unless the facet has verified standalone keyword search volume.
3. **Structured Offer Freshness**: Keep price and in-stock status synchronized across database, visual UI, and JSON-LD structured data.

## Rules
- **Rule 1 (The Facet Indexing Matrix)**:
  - High-Volume Category (`/shoes/running`): **INDEX, FOLLOW**.
  - Single High-Demand Facet (`/shoes/running/nike`): **INDEX, FOLLOW** (Dedicated landing page with unique title/H1).
  - Multi-Filter Combinations (`/shoes/running/nike?size=10&color=black&sort=price_asc`): **NOINDEX, FOLLOW** or Canonicalize to `/shoes/running/nike`.
  - Pure Sort/Pagination (`?page=2`, `?sort=newest`): **NOINDEX** or Canonicalize to base category URL.
- **Rule 2 (Product URL Structure)**:
  Product URLs must be clean, root-anchored, and independent of category path:
  - Preferred: `https://example.com/products/aer-travel-pack-3`
  - Avoid: `https://example.com/bags/backpacks/travel/products/aer-travel-pack-3` (Causes duplicate URL issues when products belong to multiple categories).
- **Rule 3 (Out-of-Stock SEO Handling)**:
  - Temporarily Out of Stock: Keep page active (`200 OK`), update schema `availability: "https://schema.org/OutOfStock"`, display email restock notification form. DO NOT 404.
  - Permanently Discontinued: If a direct replacement exists, `301 Redirect` to the successor product. Otherwise, return `410 Gone` or maintain page with links to related products.
- **Rule 4 (Internal Breadcrumbs)**:
  Every product and subcategory page must display clickable breadcrumbs matching Schema.org `BreadcrumbList`.

## Decision Criteria
```text
IF filter parameter represents a known keyword search term (e.g. brand "Nike"):
  Generate clean static URL path (/brand/nike) and allow indexing.
IF filter parameter is price range or sort order (?min=20&max=50):
  Disallow in robots.txt or apply canonical pointing to base category.
IF product variant changes only color:
  Maintain one single canonical product page with color swatch picker, or canonicalize variants to primary.
```

## Recommended Workflow
1. Map catalog taxonomy and keyword search demand per category.
2. Set up URL rewriting for valuable single-facet attributes.
3. Configure `robots.txt` disallow rules for multi-parameter queries.
4. Implement `Product` and `Offer` Schema JSON-LD on all PDPs.
5. Audit category internal link architecture to ensure deep products are reachable within 3 clicks.

## Best Practices
- Display customer reviews and ratings on product pages to enrich search result snippets.
- Use high-resolution, optimized WebP/AVIF product images with informative alt text.
- Provide clear category descriptions at the top or bottom of PLPs to establish topical authority.

## Anti-Patterns
- **The Filter Spider Trap**: Allowing search bots to crawl endless combinations of filters (`?size=9&color=red&sort=asc&page=4...`), consuming all crawl capacity.
- **404ing Out-of-Stock Products**: Deleting product pages immediately when stock drops to 0, destroying accumulated backlinks and search rankings.
- **Duplicate Variant URLs**: Generating 15 separate indexed URLs for a t-shirt in 15 different colors without canonicalization.

## Validation Checklist
- [ ] High-volume facets have clean static URLs with unique metadata.
- [ ] Deep filter combinations canonicalize to clean base category pages.
- [ ] Product URLs are independent of nested category paths.
- [ ] Out-of-stock products retain 200 status with updated Schema availability.

## Related Skills
- `skills/seo/technical-seo.md`
- `skills/seo/structured-data.md`
- `skills/ecommerce/product-pages.md`

## References
- Google Search Central: E-Commerce Best Practices — https://developers.google.com/search/docs/specialty/ecommerce
- Faceted Search UX & SEO — Baymard Institute

## Last Reviewed
2026-09-15

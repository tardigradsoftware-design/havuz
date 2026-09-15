# High-Converting Product Detail Pages (PDP) & Variant Architecture

## Purpose
Governs the architecture, layout, media presentation, and state management of Product Detail Pages (PDP), maximizing shopper clarity and conversion.

## When To Use
- When engineering e-commerce product pages, SKU variant selectors, and product gallery systems.
- When optimizing product information hierarchy, specifications, and customer reviews.

## When Not To Use
- For generic category listing pages (PLP) or landing pages without standalone purchasing options.

## Core Principles
1. **Visual Dominance**: High-resolution imagery, interactive thumbnail galleries, and zoom capabilities drive over 60% of purchase decisions in online retail.
2. **Instant Variant Synchronization**: Selecting a variant (color, size, material) must instantaneously update the price, SKU, gallery photos, and real-time stock availability without full page reload.
3. **Scannable Information Hierarchy**: Place critical purchase criteria (title, price, ratings, variant pickers, CTA) above the fold; organize deep specs and reviews in lower collapsible tabs.

## Rules
- **Rule 1 (Standard Desktop/Mobile PDP Layout)**:
  - Desktop: 2-column layout (`grid grid-cols-1 lg:grid-cols-12 gap-12`):
    - Left Column (7 cols): Sticky image gallery with vertical thumbnail rail and zoom preview.
    - Right Column (5 cols): Product title (`h1`), star rating summary, price, variant selectors, stock badge, primary "Add to Cart" button, trust guarantees, collapsible accordion details.
  - Mobile: Full-width swipeable image carousel with pagination dots, followed by title, price, variants, and sticky bottom CTA bar.
- **Rule 2 (Variant State in URL)**:
  Every selected variant option must synchronize with the URL query parameters (e.g., `/products/merino-hoodie?color=navy&size=xl`) so customers can share exact configurations.
- **Rule 3 (Stock & Scarcity Transparency)**:
  Display real-time stock status accurately:
  - In Stock: `✓ In stock, ready to ship` (Green badge).
  - Low Stock: `Low stock: Only 3 remaining` (Amber badge).
  - Out of Stock: `Out of stock` (Muted badge; disable primary CTA and display email restock notification form).
- **Rule 4 (Clear Delivery & Return Expectations)**:
  Display estimated delivery date directly next to the primary button (e.g., "Order within 4 hrs for delivery by Thursday, Sep 18").

## Decision Criteria
```text
IF product has multiple images:
  Render responsive thumbnail rail with active border indicator; enable pinch-to-zoom on mobile.
IF variant combination is invalid or out of stock:
  Visually strike through option button with diagonal slash and disable click.
IF shopper selects a color swatch:
  Automatically filter the image gallery to display photos of that specific color variant.
```

## Recommended Workflow
1. Define product data schema with variants, options, images, and inventory counts.
2. Build responsive gallery component using `next/image` with high-resolution source.
3. Implement interactive variant picker with URL query synchronization via `nuqs`.
4. Assemble product info column with structured price, discounts, and badges.
5. Embed Schema.org `Product` and `Offer` JSON-LD.
6. Verify layout stability across desktop and mobile screens.

## Best Practices
- Show discounted prices clearly with strikethrough original price and discount percentage badge (`Save 25%`).
- Include a size guide modal or chart adjacent to clothing size selectors.
- Embed customer review aggregation with filterable ratings and customer-submitted photos.

## Anti-Patterns
- **Unresponsive Variant Clashing**: Allowing a user to select "Size: XL" and "Color: Red" only to inform them on checkout that this combination does not exist.
- **Microscopic Product Photos**: Displaying product photos at 300x300px with no zoom, leaving shoppers unable to inspect material texture.
- **Hiding Shipping Costs**: Concealing delivery fees until the end of the checkout funnel.

## Validation Checklist
- [ ] Variant selections update URL query parameters seamlessly.
- [ ] Product images are optimized with `next/image` and high-resolution zoom.
- [ ] Stock availability is displayed explicitly with low-stock warnings.
- [ ] Page includes Schema.org `Product` JSON-LD.

## Related Skills
- `skills/ecommerce/ecommerce-ux.md`
- `skills/seo/ecommerce-seo.md`
- `skills/performance/image-optimization.md`

## References
- Baymard Institute Product Page UX Benchmarks
- Shopify Hydrogen Product Page Patterns — https://shopify.dev/docs/custom-storefronts/hydrogen

## Last Reviewed
2026-09-15

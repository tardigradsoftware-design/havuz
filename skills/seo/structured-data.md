# Structured Data & Schema.org JSON-LD Implementation

## Purpose
Enables search engines to unambiguously understand page content and unlocks Google Rich Results (Star Ratings, FAQ accordions, Breadcrumb trails, Product prices, Organization logos).

## When To Use
- In all commercial websites, corporate presences, e-commerce product pages, documentation, and blog articles.
- When marking up organizations, products, FAQs, articles, and breadcrumbs.

## When Not To Use
- For content that is hidden from regular users or deceptive (violates Google spam policies).

## Core Principles
1. **JSON-LD Format Exclusively**: Implement structured data using Schema.org JSON-LD scripts (`<script type="application/ld+json">`). Avoid deprecated Microdata or RDFa in HTML attributes.
2. **Strict Entity Parity**: Every claim in the structured data JSON-LD must be visibly present and readable by human users on the webpage.
3. **Validation Against Google Rich Results**: Test all schemas against Google's Rich Results schema requirements.

## Rules
- **Rule 1 (Organization Schema on Homepage)**:
  Homepage must declare the official organization entity:
  ```json
  {
    "@context": "https://schema.org",
    "@type": "Organization",
    "name": "Acme SaaS",
    "url": "https://acme.com",
    "logo": "https://acme.com/logo.png",
    "sameAs": [
      "https://twitter.com/acme",
      "https://github.com/acme",
      "https://linkedin.com/company/acme"
    ],
    "contactPoint": {
      "@type": "ContactPoint",
      "telephone": "+1-800-555-0199",
      "contactType": "customer service"
    }
  }
  ```
- **Rule 2 (Product & Offer Schema)**:
  Product detail pages must include `Product`, `Offers`, and `AggregateRating`:
  ```json
  {
    "@context": "https://schema.org",
    "@type": "Product",
    "name": "Ergonomic Mechanical Keyboard",
    "image": ["https://example.com/photos/keyboard.jpg"],
    "description": "Custom mechanical keyboard with hot-swappable switches.",
    "sku": "KB-ERG-01",
    "brand": {
      "@type": "Brand",
      "name": "KeyCraft"
    },
    "offers": {
      "@type": "Offer",
      "url": "https://example.com/products/ergonomic-keyboard",
      "priceCurrency": "USD",
      "price": "149.00",
      "priceValidUntil": "2027-12-31",
      "itemCondition": "https://schema.org/NewCondition",
      "availability": "https://schema.org/InStock"
    }
  }
  ```
- **Rule 3 (BreadcrumbList Schema)**:
  Hierarchical category and article pages must provide breadcrumb structured data:
  ```json
  {
    "@context": "https://schema.org",
    "@type": "BreadcrumbList",
    "itemListElement": [
      {
        "@type": "ListItem",
        "position": 1,
        "name": "Home",
        "item": "https://example.com"
      },
      {
        "@type": "ListItem",
        "position": 2,
        "name": "Blog",
        "item": "https://example.com/blog"
      },
      {
        "@type": "ListItem",
        "position": 3,
        "name": "AI Architecture",
        "item": "https://example.com/blog/ai-architecture"
      }
    ]
  }
  ```
- **Rule 4 (React Component Helper Pattern)**:
  Inject JSON-LD safely in Next.js Server Components:
  ```tsx
  export function JsonLd({ data }: { data: Record<string, unknown> }) {
    return (
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(data).replace(/</g, '\u003c') }}
      />
    );
  }
  ```

## Decision Criteria
```text
IF page is e-commerce product:
  Inject Product + Offer schema with price, currency, availability, and image.
IF page has Accordion FAQ:
  Inject FAQPage schema matching question and answer text exactly.
IF page is long-form article:
  Inject Article schema with headline, author, datePublished, and dateModified.
```

## Recommended Workflow
1. Select appropriate Schema.org types for page content.
2. Structure JSON-LD matching page visual elements.
3. Embed JSON-LD via Server Component script tag.
4. Validate schema using Google Rich Results Test tool.
5. Deploy and monitor Google Search Console enhancements tab.

## Best Practices
- Always use ISO 8601 formatting for dates (`"2026-09-15T12:00:00Z"`).
- Provide absolute HTTPS URLs for all schema images and links.
- Escape opening angle brackets (`<`) to prevent JSON-LD XSS injections.

## Anti-Patterns
- **Ghost Schemas**: Declaring fake 5-star ratings or FAQ questions in JSON-LD that are not visible anywhere on the actual webpage.
- **Microdata Inlining**: Scattering microdata attributes throughout 40 HTML tags instead of a clean, centralized JSON-LD block.
- **Invalid Currency Codes**: Writing `"currency": "$"` instead of ISO 4217 code `"priceCurrency": "USD"`.

## Validation Checklist
- [ ] Schema validates without errors on Google Rich Results Test.
- [ ] All structured data values match visible on-page content.
- [ ] Dates follow ISO 8601 format.
- [ ] Script tag uses sanitized JSON-LD helper.

## Related Skills
- `skills/seo/technical-seo.md`
- `skills/seo/ecommerce-seo.md`
- `skills/ecommerce/product-pages.md`

## References
- Google Search Central: Structured Data General Guidelines — https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data
- Schema.org Official Documentation — https://schema.org/

## Last Reviewed
2026-09-15

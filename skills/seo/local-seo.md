# Local SEO, Geolocation & Multi-Location Architecture

## Purpose
Drives high-intent local customer discovery and map pack visibility for physical businesses, service-area enterprises, and multi-location franchises.

## When To Use
- For regional service providers, brick-and-mortar storefronts, clinics, restaurants, and agency branches.
- When creating location landing pages, store locators, and LocalBusiness Schema markup.

## When Not To Use
- For 100% digital, borderless SaaS products with no physical presence or regional service boundaries.

## Core Principles
1. **NAP Consistency (Name, Address, Phone)**: The business name, street address, and phone number must be identical across the website, Google Business Profile, and directories.
2. **Dedicated Location Pages**: Each physical location must have its own unique, indexable URL with unique local content—never combine 10 locations into a single generic page.
3. **Local Schema Completeness**: Every location page must embed comprehensive `LocalBusiness` Schema with precise geographic coordinates.

## Rules
- **Rule 1 (NAP Format Standard)**:
  Present business details in HTML footer or header using standard schema-compatible formatting:
  ```html
  <address class="not-italic">
    <strong>Acme Dental Seattle</strong><br />
    123 Pine Street, Suite 400<br />
    Seattle, WA 98101<br />
    <a href="tel:+12065550144">(206) 555-0144</a>
  </address>
  ```
- **Rule 2 (LocalBusiness Schema.org JSON-LD)**:
  Location pages must include precise schema:
  ```json
  {
    "@context": "https://schema.org",
    "@type": "Dentist",
    "name": "Acme Dental Seattle",
    "image": "https://example.com/photos/seattle-clinic.jpg",
    "telephone": "+12065550144",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "123 Pine Street, Suite 400",
      "addressLocality": "Seattle",
      "addressRegion": "WA",
      "postalCode": "98101",
      "addressCountry": "US"
    },
    "geo": {
      "@type": "GeoCoordinates",
      "latitude": 47.6062,
      "longitude": -122.3321
    },
    "openingHoursSpecification": [
      {
        "@type": "OpeningHoursSpecification",
        "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
        "opens": "08:00",
        "closes": "17:00"
      }
    ],
    "url": "https://example.com/locations/seattle"
  }
  ```
- **Rule 3 (Local Content Distinctiveness)**:
  Never duplicate identical boilerplate copy across location pages. Each location page must feature:
  - Local staff profiles / practitioners.
  - Neighborhood driving directions and parking instructions.
  - Genuine local customer testimonials.
  - Interactive Google Maps embed.

## Decision Criteria
```text
IF business has multiple physical branches:
  Create directory hierarchy: /locations/ (index) -> /locations/city-name (leaf page).
IF business serves a region without physical customer storefront:
  Use ServiceAreaBusiness schema with areaServed geographical boundaries.
```

## Recommended Workflow
1. Establish standard NAP formatting and phone tracking protocols.
2. Scaffold individual location pages with localized metadata.
3. Implement `LocalBusiness` JSON-LD with geo coordinates.
4. Embed localized Google Map iframe.
5. Verify NAP matches official Google Business Profile exactly.

## Best Practices
- Make phone numbers clickable links (`tel:+1...`) for seamless mobile dialing.
- Include opening hours visibly on page and inside Schema markup.
- Add city and neighborhood names naturally to `<title>` and `<h1>` tags (e.g., `Dentist in Downtown Seattle, WA | Acme Dental`).

## Anti-Patterns
- **City-Stuffed Doorway Pages**: Programmatically generating 500 identical pages for every small suburb without providing real local value.
- **Mismatched Phone Numbers**: Showing one phone number on the website and a different number on Google Maps.
- **Images Without Geo Context**: Stock photos of corporate buildings instead of real photographs of the actual local store or team.

## Validation Checklist
- [ ] NAP matches Google Business Profile character-for-character.
- [ ] `LocalBusiness` JSON-LD includes valid latitude and longitude.
- [ ] Phone numbers use `tel:` clickable links.
- [ ] Each location page has distinct, localized content.

## Related Skills
- `skills/seo/on-page-seo.md`
- `skills/seo/structured-data.md`
- `skills/ui/responsive-design.md`

## References
- Google Business Profile Guidelines — https://support.google.com/business/answer/3038177
- Schema.org LocalBusiness Documentation — https://schema.org/LocalBusiness

## Last Reviewed
2026-09-15

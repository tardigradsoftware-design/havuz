# Technical SEO, Crawlability & Canonicalization

## Purpose
Guarantees that search engine web crawlers (Googlebot, Bingbot) can efficiently discover, crawl, render, and index web pages with optimal crawl budget and zero duplicate content penalties.

## When To Use
- In all public websites, corporate landing pages, blogs, and e-commerce platforms.
- When configuring `robots.txt`, dynamic `sitemap.xml`, canonical URLs, and HTTP response headers.

## When Not To Use
- Inside private, authenticated SaaS application portals (e.g., `/app`, `/dashboard`), which must be protected with `noindex, nofollow`.

## Core Principles
1. **Unambiguous Canonicals**: Every indexable page must declare an absolute self-referencing canonical URL to prevent duplication from query parameters or trailing slashes.
2. **Server-Rendered Content for Search**: Critical content, metadata, and internal links must exist in the initial server-rendered HTML response—never hide essential text behind client-only fetch cascades.
3. **Clean Crawl Directives**: Direct crawlers precisely via `robots.txt` and sitemaps while keeping private or low-value facet pages disallowed.

## Rules
- **Rule 1 (Canonical Tag Enforcement)**:
  Every public page must include an explicit, absolute canonical link:
  ```html
  <link rel="canonical" href="https://example.com/products/wireless-keyboard" />
  ```
  In Next.js App Router metadata:
  ```typescript
  export const metadata: Metadata = {
    metadataBase: new URL('https://example.com'),
    alternates: {
      canonical: '/products/wireless-keyboard',
    },
  };
  ```
- **Rule 2 (Dynamic Sitemap Generation)**:
  Implement `app/sitemap.ts` to dynamically serve an XML sitemap reflecting all published routes with `<lastmod>` timestamps:
  ```typescript
  import { MetadataRoute } from 'next';

  export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
    const posts = await getPublishedPosts();
    const postUrls = posts.map((post) => ({
      url: `https://example.com/blog/${post.slug}`,
      lastModified: post.updatedAt,
      changeFrequency: 'weekly' as const,
      priority: 0.8,
    }));

    return [
      { url: 'https://example.com', lastModified: new Date(), priority: 1.0 },
      ...postUrls,
    ];
  }
  ```
- **Rule 3 (Robots Configuration Standard)**:
  Implement `app/robots.ts` with explicit indexing boundaries:
  ```typescript
  import { MetadataRoute } from 'next';

  export default function robots(): MetadataRoute.Robots {
    return {
      rules: [
        {
          userAgent: '*',
          allow: '/',
          disallow: ['/api/', '/dashboard/', '/admin/', '/checkout/', '/*?*'],
        },
      ],
      sitemap: 'https://example.com/sitemap.xml',
    };
  }
  ```
- **Rule 4 (Private Area Protection)**:
  All authenticated dashboard or checkout pages must set:
  `<meta name="robots" content="noindex, nofollow" />`.
- **Rule 5 (Status Code Integrity)**:
  - Deleted pages permanently removed must return `410 Gone` or `301 Redirect` to the most relevant equivalent page.
  - Never return `200 OK` for missing pages (Soft 404 penalty).

## Decision Criteria
```text
IF page is private or behind login:
  Set robots: { index: false, follow: false }.
IF page has URL parameters (sorting, pagination):
  Set canonical URL to the clean base URL without sort parameters.
IF migrating or moving an old URL:
  Implement permanent HTTP 301 Redirect; DO NOT use temporary 302 or JavaScript redirects.
```

## Recommended Workflow
1. Define `metadataBase` in root layout (`app/layout.tsx`).
2. Implement `app/robots.ts` and `app/sitemap.ts`.
3. Configure canonical tags dynamically across all route templates.
4. Verify HTTP status codes (`curl -I https://example.com/page`).
5. Audit with Google Search Console or Lighthouse SEO audit.

## Best Practices
- Keep sitemaps under 50,000 URLs and 50MB (split into sitemap indexes if exceeding).
- Standardize on lowercase URLs with hyphens (`/best-laptop-stands`, not `/Best_Laptop_Stands`).
- Ensure no mixed HTTP/HTTPS links exist on the site; enforce HTTPS strictly with HSTS.

## Anti-Patterns
- **Soft 404s**: Rendering a "Page Not Found" visual message while returning HTTP 200 status code.
- **Missing Canonicals**: Allowing `example.com/page`, `example.com/page/`, and `example.com/page?utm_source=fb` to be indexed as 3 distinct pages.
- **Blocking CSS/JS in robots.txt**: Disallowing crawler access to CSS or JS files, which prevents search engines from properly rendering the page.

## Validation Checklist
- [ ] `robots.txt` is accessible and points to sitemap.
- [ ] `sitemap.xml` is valid and contains updated `<lastmod>` dates.
- [ ] Every indexable page contains an absolute canonical URL.
- [ ] Private dashboard routes are marked with `noindex, nofollow`.

## Related Skills
- `skills/seo/on-page-seo.md`
- `skills/seo/structured-data.md`
- `skills/performance/core-web-vitals.md`

## References
- Google Search Central: Technical SEO Guide — https://developers.google.com/search/docs/crawling-indexing
- Sitemaps XML Protocol Specification — https://www.sitemaps.org/protocol.html

## Last Reviewed
2026-09-15

# Multi-Layer Caching Architecture & Stale-While-Revalidate

## Purpose
Establishes multi-tier caching strategies across Browser, CDN Edge, Server Data Cache, and In-Memory stores to achieve lightning-fast response times while preventing stale data inconsistencies.

## When To Use
- When configuring HTTP headers, CDN caching rules, Next.js Data Cache, and Redis caching layers.
- When architecting mutation workflows to guarantee instant cache revalidation.

## When Not To Use
- For highly sensitive financial transactions or real-time security revocation checks that require strict zero-cache freshness.

## Core Principles
1. **Cache at the Nearest Edge**: Serve content as close to the physical user as possible (Browser Cache -> CDN Edge -> Server Memory -> Database).
2. **Explicit Invalidation Over Timeouts**: Do not rely solely on arbitrary TTL expiration; explicitly invalidate cached tags (`revalidateTag`) immediately when data mutates.
3. **Never Cache Authenticated PII Globally**: Never allow CDN or public caches to store responses containing private user tokens, personal profile data, or tenant-specific records.

## Rules
- **Rule 1 (HTTP Cache-Control Directives)**:
  - Static Assets (`/static/*`, hashed JS/CSS, images):
    `Cache-Control: public, max-age=31536000, immutable`
  - Public Dynamic Content (Blog, Catalog):
    `Cache-Control: public, s-maxage=3600, stale-while-revalidate=86400`
  - Private User Data (Dashboard, Profile, Settings):
    `Cache-Control: private, no-cache, no-store, must-revalidate`
- **Rule 2 (Next.js Data Cache Tagging)**:
  Tag data queries explicitly so mutations can trigger surgical revalidation:
  ```typescript
  export async function getProduct(slug: string) {
    const res = await fetch(`https://api.example.com/products/${slug}`, {
      next: { tags: [`product-${slug}`, 'products'] },
    });
    return res.json();
  }

  // In Mutation Action:
  export async function updateProduct(slug: string, data: ProductInput) {
    await db.product.update({ ... });
    revalidateTag(`product-${slug}`);
  }
  ```
- **Rule 3 (Stale-While-Revalidate Pattern)**:
  Use SWR semantics to serve instantaneous cached responses immediately while asynchronously revalidating in the background.
- **Rule 4 (Vary Header Enforcement)**:
  When caching responses that adapt based on client headers (e.g., mobile layout, compression, authentication state), always include the `Vary` header:
  `Vary: Accept-Encoding, Cookie`.

## Decision Criteria
```text
IF resource has content-hashed filename (/main.a89f7b.js):
  Set max-age=31536000, immutable.
IF endpoint returns personalized user dashboard data:
  Set private, no-store.
IF public page changes occasionally:
  Use Next.js ISR (Incremental Static Regeneration) with revalidate: 3600 or on-demand revalidation.
```

## Recommended Workflow
1. Classify every route and asset as Static, Public Dynamic, or Private Dynamic.
2. Apply appropriate `Cache-Control` headers in middleware or route handlers.
3. Instrument server queries with cache tags.
4. Hook mutation handlers to revalidate specific tags.
5. Inspect response headers using `curl -I` to verify cache hits (`cf-cache-status: HIT` or `x-vercel-cache: HIT`).

## Best Practices
- Use Redis (Upstash / MemoryStore) for rate limiting and serverless session caching.
- Warm critical caches post-deployment via automated crawl or webhook triggers.
- Ensure all cache keys incorporate tenant IDs in multi-tenant environments.

## Anti-Patterns
- **The Accidental Public Cache**: Caching `/api/user` with `public, max-age=3600`, causing User B to see User A's private dashboard data.
- **Cache Invalidation Blindness**: Updating a product's price in the database but forgetting to clear the CDN cache, leaving wrong prices visible for hours.
- **Over-Caching HTML**: Serving un-hashed index HTML files with `immutable`, preventing users from receiving application updates.

## Validation Checklist
- [ ] Static assets have `max-age=31536000, immutable`.
- [ ] Private routes explicitly set `private, no-store`.
- [ ] Dynamic public data uses cache tagging with on-demand invalidation.
- [ ] Response headers verify cache hits on CDN edge.

## Related Skills
- `skills/performance/core-web-vitals.md`
- `skills/frontend/nextjs.md`
- `skills/backend/database.md`

## References
- MDN HTTP Caching Guide — https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching
- Next.js Caching and Revalidating Documentation — https://nextjs.org/docs/app/building-your-application/caching

## Last Reviewed
2026-09-15

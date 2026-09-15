# Modern Image Optimization & Responsive Delivery

## Purpose
Establishes modern image processing pipelines, responsive source-set delivery, next-gen format conversion, and zero-shift loading strategies to minimize bandwidth and accelerate page rendering.

## When To Use
- In every web page containing images, banners, product photos, avatars, and icons.
- When configuring CDN media storage, image upload transformations, and responsive galleries.

## When Not To Use
- For pure vector icons (use SVG / Lucide icons) or CSS geometric patterns.

## Core Principles
1. **Next-Gen Formats**: Deliver images in **AVIF** (preferred) or **WebP** formats; never deliver uncompressed raw PNG or JPEG to modern browsers.
2. **Exact Resolution Matching**: Never serve a 2400px desktop image to a 375px mobile device. Provide responsive `srcset` and `sizes`.
3. **Zero Layout Shift (CLS)**: Always provide intrinsic aspect ratio dimensions (`width` and `height`) so the browser reserves space prior to download.

## Rules
- **Rule 1 (Next.js Image Primitive Requirement)**:
  Always use `next/image` rather than standard HTML `<img>`:
  ```tsx
  import Image from 'next/image';

  export function HeroImage() {
    return (
      <div className="relative aspect-video w-full max-w-4xl overflow-hidden rounded-xl">
        <Image
          src="/images/hero-banner.jpg"
          alt="Product interface overview showing analytics charts"
          fill
          priority
          sizes="(max-width: 768px) 100vw, (max-width: 1200px) 80vw, 1200px"
          className="object-cover"
        />
      </div>
    );
  }
  ```
- **Rule 2 (The Priority Rule)**:
  - **Above the Fold**: The primary LCP image MUST have `priority={true}`.
  - **Below the Fold**: All images below the fold MUST use default lazy loading (`loading="lazy"`).
  - Never add `priority` to images below the fold (wastes initial bandwidth).
- **Rule 3 (The Sizes Attribute Requirement)**:
  Whenever using `fill`, you must provide an accurate `sizes` attribute. Omitting `sizes` causes the browser to download a full-viewport 4K image even on mobile screens.
- **Rule 4 (Blur Placeholder Standards)**:
  For static images, use `placeholder="blur"`. For dynamic remote images, generate a compact low-quality image placeholder (LQIP) or base64 blur data URL.

## Decision Criteria
```text
IF image is above-the-fold hero:
  Add `priority={true}`; DO NOT lazy load.
IF image is vector/icon:
  Use inline SVG or Lucide component; DO NOT render as raster PNG/JPEG.
IF image is uploaded by user:
  Process on upload: strip EXIF metadata, resize to max 2048px, compress to WebP/AVIF.
```

## Recommended Workflow
1. Select appropriate source image with high fidelity.
2. Place static assets in `/public/` or host on image CDN (Cloudinary, Cloudflare Images).
3. Implement using `next/image` with explicit aspect ratio wrapper (`aspect-square`, `aspect-video`).
4. Define responsive `sizes` string matching layout breakpoints.
5. Verify network tab: format should be `image/avif` or `image/webp` with payload < 150kB.

## Best Practices
- Strip camera EXIF data and geolocation metadata from user-uploaded images for privacy.
- Set CDN cache TTL on images to 1 year (`Cache-Control: public, max-age=31536000, immutable`).
- Use SVG for simple logos and icons to guarantee infinite crispness at tiny file weights.

## Anti-Patterns
- **Raw Uncompressed Uploads**: Serving a 12MB raw DSLR JPEG straight from an S3 bucket to a mobile user.
- **Missing Sizes on Fill**: `<Image fill src="..." />` without `sizes`, forcing a 3840px image download onto an iPhone.
- **CSS Background Image for Hero**: `style={{ backgroundImage: 'url(/hero.jpg)' }}` (Cannot be preloaded by browser HTML scanner, delaying LCP by up to 2 seconds).

## Validation Checklist
- [ ] No unoptimized HTML `<img>` tags used for raster images.
- [ ] LCP image has `priority={true}` and preloads in document head.
- [ ] All `fill` images define explicit `sizes` attributes.
- [ ] Images deliver in AVIF or WebP format.

## Related Skills
- `skills/performance/core-web-vitals.md`
- `skills/frontend/nextjs.md`
- `skills/ecommerce/product-pages.md`

## References
- Next.js Image Optimization Documentation — https://nextjs.org/docs/app/building-your-application/optimizing/images
- Google web.dev Image Optimization Guide — https://web.dev/fast/#optimize-your-images

## Last Reviewed
2026-09-15

# Next.js 15 App Router Architecture

## Purpose
Governs the architecture, directory structure, data fetching, caching, streaming, and optimization of production Next.js applications using the App Router.

## When To Use
- When developing modern full-stack web applications, SaaS platforms, corporate sites, or headless e-commerce storefronts.
- When organizing routing, layouts, metadata, API route handlers, and server actions.

## When Not To Use
- For legacy Pages Router (`pages/`) applications unless undergoing explicit step-by-step migration.

## Core Principles
1. **Streaming SSR by Default**: Deliver instant UI shells with progressive content streaming via React Suspense.
2. **Server/Client Boundary Separation**: Pass serialized data across boundaries; never leak Node.js runtime code or private environment variables to client components.
3. **Colocation of Concerns**: Colocate components, test files, and utilities with their respective routes in `app/(group)/route/`.

## Rules
- **Rule 1 (App Directory Hierarchy)**:
  Follow canonical file conventions:
  - `layout.tsx`: Persistent shell (sidebar, navigation) preserving state on route change.
  - `page.tsx`: Unique UI route endpoint.
  - `loading.tsx`: Instant fallback skeleton automatically wrapped in Suspense.
  - `error.tsx`: Client-side error boundary with reset mechanism.
  - `not-found.tsx`: Custom 404 response handler.
- **Rule 2 (Server Action Security)**:
  Every Server Action must validate authentication and parse input parameters with Zod before executing business logic:
  ```tsx
  'use server';

  import { z } from 'zod';
  import { auth } from '@/lib/auth';

  const InputSchema = z.object({ title: z.string().min(3).max(100) });

  export async function createProject(formData: FormData) {
    const session = await auth();
    if (!session?.user) throw new Error('Unauthorized');

    const result = InputSchema.safeParse({ title: formData.get('title') });
    if (!result.success) {
      return { error: result.error.flatten().fieldErrors };
    }

    await db.project.create({ data: { title: result.data.title, userId: session.user.id } });
    revalidatePath('/dashboard/projects');
  }
  ```
- **Rule 3 (Static vs Dynamic Optimization)**:
  - Static pages (marketing, docs): Pre-render at build time.
  - Dynamic pages (personalized dashboard): Render on request using `export const dynamic = 'force-dynamic'` or dynamic functions (`cookies()`, `headers()`).
- **Rule 4 (Asset Optimization)**:
  - Images: Always use `next/image` with `sizes`, `alt`, and `priority` on the LCP image.
  - Fonts: Always use `next/font` (`next/font/google` or `next/font/local`) to eliminate layout shift and external network calls.
  - Metadata: Export static `metadata` object or dynamic `generateMetadata` function for SEO.

## Decision Criteria
```text
IF route contains sensitive client-specific data:
  Fetch data inside Server Component using cookies()/auth(); DO NOT cache globally.
IF route is public content updated occasionally (e.g., Blog):
  Use Incremental Static Regeneration (ISR): `export const revalidate = 3600`.
IF client component needs data from server:
  Fetch data in Server Component parent and pass as props to Client Component child.
```

## Recommended Workflow
1. Structure route groups logically: `app/(marketing)/`, `app/(auth)/`, `app/(dashboard)/`.
2. Configure root `app/layout.tsx` with fonts, theme provider, and global metadata.
3. Scaffold `loading.tsx` skeletons for each dynamic route group.
4. Implement route pages using async Server Components.
5. Create Server Actions in dedicated `actions/` files with `'use server'` directives.
6. Verify production build output via `npm run build`.

## Best Practices
- Use `revalidatePath` or `revalidateTag` immediately after data mutations to update cached views.
- Keep Route Handlers (`app/api/*/route.ts`) exclusively for external webhooks or public third-party APIs; use Server Actions for application UI mutations.
- Use parallel routes (`@modal`, `@analytics`) and intercepting routes (`(..)photo`) for complex overlay patterns.

## Anti-Patterns
- **Using Client-Side Fetch for Internal Data**: Creating an API route in Next.js just so a Client Component can `fetch('/api/data')` instead of fetching in a Server Component.
- **Unprotected Server Actions**: Forgetting to verify user authentication inside a server action, assuming the client UI protection was sufficient.
- **Unbounded Image Sizing**: Rendering raw `<img>` tags without dimensions, triggering severe Cumulative Layout Shift (CLS).

## Validation Checklist
- [ ] Build succeeds with zero Next.js compilation or route generation errors.
- [ ] Every Server Action verifies authentication and validates input via Zod.
- [ ] Every page exports valid `metadata` or `generateMetadata`.
- [ ] All images leverage `next/image` with proper width, height, and alt attributes.

## Related Skills
- `skills/frontend/react.md`
- `skills/seo/technical-seo.md`
- `skills/performance/core-web-vitals.md`

## References
- Next.js Official Documentation — https://nextjs.org/docs
- Vercel Next.js App Router Patterns — https://nextjs.org/docs/app/building-your-application

## Last Reviewed
2026-09-15

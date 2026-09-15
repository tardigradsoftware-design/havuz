# On-Page SEO, Content Structure & Open Graph Protocol

## Purpose
Establishes best practices for keyword targeting, semantic HTML content hierarchy, title/meta tag crafting, and social sharing Open Graph cards.

## When To Use
- For every public web page, blog post, landing page, and category page.
- When generating metadata, social preview images, and internal linking strategies.

## When Not To Use
- For private user profile settings, backend API routes, or internal admin consoles.

## Core Principles
1. **Search Intent Alignment**: Match content directly to the user's specific query intent (informational, commercial, navigational, transactional).
2. **Semantic Document Outlines**: Use strict heading levels (`h1` -> `h2` -> `h3`) reflecting logical content grouping, not visual sizing.
3. **Click-Through Optimization (CTR)**: Titles and descriptions must be crafted to maximize click-through rate from search result pages without deceptive clickbait.

## Rules
- **Rule 1 (Title Tag Precision Formula)**:
  - Length: **50–60 characters** (~580 pixels max).
  - Formula: `[Primary Target Keyword] - [Secondary Context / Unique Value] | [Brand Name]`
  - Example: `AI Code Review Platform - Automate Pull Requests | CodeSense`
- **Rule 2 (Meta Description Standard)**:
  - Length: **140–160 characters**.
  - Formula: Must contain primary keyword naturally, summarize page value proposition, and conclude with an active Call-to-Action.
- **Rule 3 (Heading Tag Hierarchy)**:
  - Exactly **one `<h1>` per page**, containing the primary page topic.
  - `<h2>` tags for major conceptual sections.
  - `<h3>` tags for subsections within an `<h2>`.
  - Never skip heading levels (e.g., jumping from `h2` straight to `h4`).
- **Rule 4 (Open Graph & Social Cards)**:
  Every indexable page must export complete Open Graph and Twitter Card tags:
  ```typescript
  export const metadata: Metadata = {
    title: 'Modern Web Development Operating System',
    description: 'Comprehensive guidelines and architecture for autonomous coding agents.',
    openGraph: {
      title: 'Modern Web Development Operating System',
      description: 'Comprehensive guidelines and architecture for autonomous coding agents.',
      url: 'https://example.com/guide',
      siteName: 'CodeMaster',
      images: [
        {
          url: 'https://example.com/og/guide.jpg',
          width: 1200,
          height: 630,
          alt: 'Modern Web Development Operating System Preview',
        },
      ],
      locale: 'en_US',
      type: 'article',
    },
    twitter: {
      card: 'summary_large_image',
      title: 'Modern Web Development Operating System',
      description: 'Comprehensive guidelines and architecture for autonomous coding agents.',
      images: ['https://example.com/og/guide.jpg'],
    },
  };
  ```
- **Rule 5 (Strategic Internal Linking)**:
  Use descriptive, contextual anchor text for internal links. Never use generic anchor text like "click here", "read more", or "link".

## Decision Criteria
```text
IF title exceeds 60 characters:
  Trim to prevent Google search result truncation (...).
IF page is dynamic blog post or product:
  Generate dynamic OG image via `@vercel/og` or dynamic edge canvas.
IF image is in main content:
  Provide descriptive alt text containing target keyword naturally if relevant.
```

## Recommended Workflow
1. Perform search intent analysis for the target page.
2. Formulate title tag and meta description adhering to character limits.
3. Draft content using semantic HTML: `<main>`, `<article>`, `<header>`, `<h1>`-`<h3>`, `<p>`.
4. Add Open Graph image (1200x630px).
5. Interlink with at least 3 relevant internal pages using contextual anchor text.

## Best Practices
- Place primary keywords near the beginning of the `<h1>` and title tag.
- Add descriptive captions and image alts that describe image content contextually.
- Include a Table of Contents with jump-anchors (`#section-id`) on long-form articles (> 1500 words).

## Anti-Patterns
- **Keyword Stuffing**: Cramming keywords repeatedly into text and tags, triggering search engine spam penalties.
- **Multiple H1 Tags**: Using 3 different `<h1>` elements on a single page because they looked stylistically bold.
- **Empty or Duplicate Meta Descriptions**: Leaving meta descriptions blank or copying the same description across 50 pages.

## Validation Checklist
- [ ] Title tag length is between 50 and 60 characters.
- [ ] Meta description length is between 140 and 160 characters.
- [ ] Exactly one semantic `<h1>` tag is present.
- [ ] Complete Open Graph metadata (1200x630 image) is configured.
- [ ] All internal links use descriptive anchor text.

## Related Skills
- `skills/seo/technical-seo.md`
- `skills/seo/structured-data.md`
- `skills/ui/typography.md`

## References
- Google Search Central: Title Links & Snippets — https://developers.google.com/search/docs/appearance/title-link
- Open Graph Protocol Specification — https://ogp.me/

## Last Reviewed
2026-09-15

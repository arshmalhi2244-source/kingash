# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # start dev server at http://localhost:4321
npm run build     # production build to dist/
npm run preview   # preview the production build locally
```

There is no test suite, linter, or type-check script configured. `tsconfig.json` extends `astro/tsconfigs/strict`, so type errors will surface in the editor and during `astro build`.

## Architecture

This is an Astro 5 static site (SSG, no server runtime) styled with Tailwind CSS v4, deployed to Cloudflare Pages. There is no CMS, database, or backend — it's fully static and content-driven from one TypeScript file.

**All page content lives in `src/data/site.ts`.** This single object (`site`) contains business info, nav, services, testimonials, FAQs, pricing tiers, and service areas. Pages import `site` and render it — they contain no hardcoded copy for these sections. When asked to update text/pricing/services/testimonials/areas, edit `site.ts`, not the page templates.

**Dynamic pages are generated from `site.ts` arrays via `getStaticPaths`:**
- `src/pages/services/[slug].astro` — one page per `site.services[]` entry (uses `service.slug`)
- `src/pages/areas/[area].astro` — one page per `site.areas[]` entry (uses `area.slug`), a local-SEO landing page per city
- `src/pages/blog/*.astro` — blog posts are NOT data-driven; each is its own `.astro` file, manually added to the `posts` array in `src/pages/blog/index.astro`

Adding a new service area or service is a data-only change (add an entry to the array in `site.ts`); the route is generated automatically. Adding a blog post requires creating a new file under `src/pages/blog/` (copy an existing post) plus registering it in `blog/index.astro`, and adding the URL to `public/sitemap.xml`.

**Every page wraps content in `src/layouts/BaseLayout.astro`**, which owns the `<head>` (meta tags, Open Graph, Twitter cards, Google Fonts preload, JSON-LD schema injection) and renders `Header`/`Footer` around a `<slot />`. Pages pass `title`, `description`, `schema` (JSON-LD object or array), and optionally `canonical`/`noindex` as props. Structured data (`LocalBusiness`, `Service`, `BreadcrumbList`, `FAQPage`, etc.) is built inline in each page's frontmatter and passed to `schema`.

**Scroll-reveal animation** is a single global `IntersectionObserver` registered in `BaseLayout.astro`'s inline script — it watches for any element with the `.reveal` class and adds `.visible` when it enters the viewport. Section-level components use `.reveal` plus `.reveal-delay-{1-4}` (defined in `global.css`) for staggered entrance animations. There is no JS animation library.

**Theme tokens are defined once in `src/styles/global.css`** via Tailwind v4's `@theme` block (dark navy/black + gold/silver palette: `navy-950/900/800/700`, `gold-300..600`, `silver-300..500`). Custom utility classes for the sci-fi hero visuals (`.hero-bg`, `.hero-grid`, `.hero-orb`, `.hero-scanline`, `.hero-particles`, `.gradient-text`, `.card-hover`) also live here — reuse these rather than redefining similar effects inline.

## SEO conventions

- Every page sets JSON-LD schema appropriate to its content (LocalBusiness for area pages, Service + FAQPage for service pages, BreadcrumbList everywhere there's a breadcrumb nav).
- `public/sitemap.xml` is maintained by hand — any new page (blog post, area, service) must be added here manually; it is not auto-generated.
- `public/_headers` sets long-lived immutable caching for `/_astro/` assets and 1-month caching for images — keep this in mind if changing asset output paths.
- `site.website` (`https://kingash.ca`) is the canonical base URL used to build absolute URLs in schema markup.

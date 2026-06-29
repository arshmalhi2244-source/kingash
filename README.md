# KingAsh Marketing Agency Website

Production-ready agency website built with Astro 5 + Tailwind CSS v4. Deployed on Cloudflare Pages.

## Tech Stack

- **Framework:** Astro 5 (SSG)
- **Styling:** Tailwind CSS v4
- **Fonts:** Space Grotesk (headings) + Inter (body) via Google Fonts
- **Deployment:** Cloudflare Pages via GitHub

## Run Locally

```bash
npm install
npm run dev
```

Open `http://localhost:4321` in your browser.

Build for production:
```bash
npm run build
npm run preview   # preview the production build locally
```

## Pages

| URL | Page |
|-----|------|
| `/` | Homepage |
| `/about` | About |
| `/services` | Services overview |
| `/services/website-design` | Website Design service |
| `/services/ai-phone-representative` | AI Phone Representative |
| `/services/ai-ads-meta-ads` | AI Ads & Meta Ads |
| `/services/seo` | Local SEO |
| `/pricing` | Pricing |
| `/contact` | Contact |
| `/blog` | Blog index |
| `/blog/[slug]` | Individual blog posts (3 posts) |
| `/areas/[city]` | Location pages (15 cities across Canada) |

## Updating Content

**All site content is in one file: `src/data/site.ts`**

- Business name, phone, email → `site.name`, `site.phone`, `site.email`
- Services (name, price, description, benefits) → `site.services[]`
- Testimonials → `site.testimonials[]`
- FAQs → `site.faqs[]`
- Pricing tiers → `site.pricing[]`
- Service areas → `site.areas[]`
- Stats bar numbers → `site.stats[]`

You do not need to edit individual page files to update this content.

## Add a New Blog Post

1. Create a new file in `src/pages/blog/your-post-slug.astro`
2. Copy an existing post as a template
3. Update the title, description, date, schema, and content
4. Add it to the `posts` array in `src/pages/blog/index.astro`
5. Add the URL to `public/sitemap.xml`

## Add a New Service Area

1. Add `{ slug: "city-name", name: "City Name", province: "BC" }` to `site.areas` in `src/data/site.ts`
2. Add the URL to `public/sitemap.xml`
3. The page at `/areas/city-name` is automatically generated

## Deploy to Cloudflare Pages

### First-Time Setup

1. Push this repo to GitHub
2. Log in to [Cloudflare Pages](https://pages.cloudflare.com/)
3. Click **"Create a project"** → **"Connect to Git"**
4. Select your GitHub repository
5. Configure the build settings:
   - **Framework preset:** Astro
   - **Build command:** `npm run build`
   - **Build output directory:** `dist`
6. Click **"Save and Deploy"**

### Connect Your Domain (kingash.ca)

1. In Cloudflare Pages, go to **Custom domains**
2. Click **"Set up a custom domain"**
3. Enter `kingash.ca`
4. Cloudflare will guide you through DNS configuration
5. If your domain registrar is already Cloudflare, DNS is updated automatically

### Subsequent Deployments

Every `git push` to your `main` branch automatically triggers a new deployment. No manual steps needed.

## Performance Notes

- All assets in `/_astro/` are immutable and cached for 1 year (`public/_headers`)
- Images are served with 1-month cache headers
- Fonts are preloaded in `BaseLayout.astro`
- No heavy JavaScript libraries — only minimal inline scripts (~200 bytes)
- CSS animations only (no JS animation libraries)

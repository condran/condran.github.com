# Rebuild paulcondran.com: Jekyll → Astro

## Context

Paul's personal website (paulcondran.com) has been dormant since 2013, running Jekyll 1.x with Bootstrap 2.x on GitHub Pages. The goal is a complete rebuild as a modern, minimalist editorial site for writing about technology, creative writing, and philosophy. A type specimen page (`specimen.html`) has already been created establishing the full design system — fonts, colors, dark mode, and layout conventions. This plan executes the Astro rebuild.

---

## Project Setup

**Location:** `~/Developer/Code/paulcondran.com/` (new directory, old repo stays as archive)

```bash
npm create astro@latest paulcondran.com -- --template minimal
cd paulcondran.com
npm install @astrojs/rss @astrojs/sitemap
```

Copy assets from old site:
- `img/instagram_me.jpg` → `public/img/`
- `img/zombiehead.png` → `public/img/`
- `CNAME` → `public/CNAME` (contains `paulcondran.com`)
- `img/favicon.ico` → `public/favicon.ico`

Create `public/robots.txt` with sitemap reference.

---

## Directory Structure

```
src/
├── components/
│   ├── BaseHead.astro          # <head>: meta, fonts, OG tags, theme flash prevention
│   ├── Header.astro            # Site title (Josefin Sans) + nav (Instrument Sans)
│   ├── Footer.astro            # Copyright, colophon link, RSS link
│   ├── ThemeToggle.astro       # Dark mode pill toggle (from specimen.html)
│   ├── PostCard.astro          # Post preview: category, title, date, summary
│   ├── PostMeta.astro          # Date + reading time + category tag
│   └── PostNav.astro           # Previous/next post links
├── content/
│   ├── config.ts               # Zod schema for writing collection
│   └── writing/
│       ├── github-game-off-lessons.md    # Migrated post
│       └── java-ognl-introspection.md    # Migrated post
├── layouts/
│   ├── BaseLayout.astro        # HTML shell: head, header, main, footer
│   └── PostLayout.astro        # Article layout: title, subtitle, meta, drop cap body
├── pages/
│   ├── index.astro             # Homepage: hero + recent writing
│   ├── about.astro             # Bio, photo, interests
│   ├── colophon.astro          # Type system, tech stack, design philosophy
│   ├── feed.xml.ts             # RSS feed via @astrojs/rss
│   └── writing/
│       ├── index.astro         # All posts, newest first, topic nav
│       ├── technology.astro    # Filtered: technology posts
│       ├── creative-writing.astro
│       ├── philosophy.astro
│       └── [...slug].astro     # Individual post pages
└── styles/
    ├── global.css              # Reset, CSS custom properties (tokens), base elements
    ├── components.css          # Header, footer, toggle, post card, topic nav
    ├── article.css             # Article title, body, drop cap, code, pull quotes
    └── utilities.css           # .container, .sr-only, fadeUp animation, transitions
```

---

## Configuration

**`astro.config.mjs`:**
- `site: 'https://paulcondran.com'`
- `output: 'static'`
- `build.format: 'directory'` (clean URLs)
- Shiki dual themes: `github-light` / `github-dark`
- Integrations: `@astrojs/sitemap`

**`src/content/config.ts`** — Content collection schema:
```ts
schema: z.object({
  title: z.string(),
  summary: z.string(),
  date: z.coerce.date(),
  updated: z.coerce.date().optional(),
  category: z.enum(['technology', 'creative-writing', 'philosophy']),
  tags: z.array(z.string()).default([]),
  image: z.string().optional(),
  draft: z.boolean().default(false),
})
```

---

## CSS Architecture

All styles extracted and reorganized from `specimen.html` (the single source of truth):

**`global.css`** — Design tokens from specimen `:root` and `[data-theme="dark"]` blocks:
- Light: `--bg: #FAFAF8`, `--text: #1a1a1a`, `--accent: #c4653a`
- Dark: `--bg: #161514`, `--text: #e8e4df`, `--accent: #d4825e`
- Font vars: `--font-display`, `--font-body`, `--font-ui`, `--font-accent`, `--font-code`
- Base HTML element styles for Markdown-rendered content (`a`, `p`, `h1-h6`, `code`, `blockquote`, etc.)

**`components.css`** — Theme toggle (specimen lines 58-120), site header, footer, post cards (adapted from specimen `.card` styles), category tags, topic nav

**`article.css`** — Article title (Instrument Serif 3.2rem), subtitle (italic), meta line, body text (Newsreader 1.1rem, line-height 1.7), drop cap `::first-letter`, code blocks (Shiki + dark mode override), pull quotes, captions

**`utilities.css`** — `.container` (max-width 900px), `.sr-only`, `fadeUp` keyframes, transition helpers

Custom CSS throughout — no Tailwind.

---

## Dark Mode (3 layers)

1. **Flash prevention** — `<script is:inline>` in `BaseHead.astro` `<head>` reads `localStorage` / `prefers-color-scheme` and sets `data-theme` before first paint
2. **CSS tokens** — `[data-theme="dark"]` overrides all `--custom-properties` in `global.css`; smooth 0.4s transitions on `body`
3. **Toggle widget** — `ThemeToggle.astro`: pill track + sliding thumb, persists to `localStorage`, keyboard accessible (`role="switch"`), respects system preference changes

Shiki code blocks: use CSS override `[data-theme="dark"] .astro-code span { color: var(--shiki-dark) !important; }` to sync with theme toggle.

---

## Pages

| Page | Route | Key elements |
|------|-------|-------------|
| **Homepage** | `/` | Hero with site title (Josefin Sans), tagline (Newsreader 300), 6 most recent posts as `PostCard` components |
| **Writing** | `/writing/` | All posts newest-first, topic nav links (All / Technology / Creative Writing / Philosophy) |
| **Topic pages** | `/writing/technology/` etc. | Filtered post lists with active topic nav state |
| **Post** | `/writing/[slug]/` | `PostLayout`: display title, italic subtitle, meta bar, drop-cap body, prev/next nav |
| **About** | `/about/` | Profile photo, bio, interests |
| **Colophon** | `/colophon/` | Type system docs, tech stack, design philosophy |
| **RSS** | `/feed.xml` | Via `@astrojs/rss`, all non-draft posts |

---

## Content Migration

Two existing posts get new frontmatter and move into `src/content/writing/`:

**`github-game-off-lessons.md`** — Add: `date: 2013-03-20`, `category: technology`, `tags: ["game-development", "javascript"]`, `image: /img/zombiehead.png`. Body unchanged.

**`java-ognl-introspection.md`** — Add: `date: 2013-03-24`, `category: technology`, `tags: ["java", "ognl", "reflection"]`. Body unchanged. Java code blocks will get Shiki syntax highlighting automatically.

---

## Deployment

**Platform: Cloudflare Pages** (replaces GitHub Pages)

Cloudflare Pages is preferred over GitHub Pages for this project:
- Global edge CDN (300+ locations) vs GitHub Pages' limited CDN
- No bandwidth/request limits on free tier
- `_headers` file for HTTP security headers (improves Lighthouse Best Practices to 100)
- Built-in privacy-respecting Web Analytics (zero JS required)
- Automatic preview deployments on every branch/PR
- Direct GitHub repo integration — no workflow YAML needed

**Setup steps:**
1. Push repo to GitHub (any name — `paulcondran.com` is clean)
2. Log in to dash.cloudflare.com → Workers & Pages → Create → Pages → Connect to Git
3. Select repo, set build command: `npm run build`, output directory: `dist`
4. Cloudflare auto-detects Astro — no further build config needed
5. Add custom domain `paulcondran.com` in the Pages project → Custom domains tab
6. Update DNS: Cloudflare will prompt to add a CNAME record pointing to `<project>.pages.dev`

**No `public/CNAME` file needed** — Cloudflare Pages uses its own dashboard for custom domain binding.

**`public/_headers`** — HTTP security headers (Cloudflare Pages processes this file):
```
/*
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: camera=(), microphone=(), geolocation=()
  X-XSS-Protection: 1; mode=block

/feed.xml
  Content-Type: application/rss+xml; charset=utf-8
```

**`public/_redirects`** — optional, for future URL migrations:
```
# Example: /blog/* → /writing/:splat 301
```

**`astro.config.mjs`** — no `base` path needed (Cloudflare Pages serves from root with custom domain). The `site` value stays `https://paulcondran.com`.

---

## Implementation Order

| Phase | Work |
|-------|------|
| **1. Scaffold** | Create project, install deps, copy assets, CNAME, .gitignore, robots.txt |
| **2. Config** | `astro.config.mjs`, `src/content/config.ts` |
| **3. Styles** | `global.css` → `utilities.css` → `components.css` → `article.css` (all from specimen.html) |
| **4. Components** | BaseHead → ThemeToggle → Header → Footer → PostCard → PostMeta → PostNav |
| **5. Layouts** | BaseLayout → PostLayout |
| **6. Pages** | Homepage → Writing index → Topic pages → Post page → About → Colophon |
| **7. Content** | Migrate 2 posts, write RSS feed endpoint |
| **8. Deploy** | Push to GitHub, connect repo to Cloudflare Pages, configure custom domain, add `_headers` file |
| **9. Verify** | Full testing pass (see below) |

---

## Verification

1. `npm run dev` — all pages render at `localhost:4321`
2. Dark mode: toggle works, persists on refresh, no flash, code blocks switch themes
3. Typography: each font renders in its correct role (check against specimen.html)
4. Responsive: 700px breakpoint — font size reduces, layouts stack, toggle moves to bottom-left
5. `npm run build && npx serve dist` — all links work, no 404s, RSS valid, CNAME present
6. Lighthouse: target 95+ perf, 100 a11y, 100 best practices, 100 SEO
7. Deploy to Cloudflare Pages, verify live at paulcondran.com and at `<project>.pages.dev` preview URL

---

## Key Reference Files

- **Design system source:** `/Users/paul/Developer/Code/condran.github.com/specimen.html`
- **Post to migrate:** `/Users/paul/Developer/Code/condran.github.com/_posts/2013-03-20-what-i-learnt-doing-github-game-off.md`
- **Post to migrate:** `/Users/paul/Developer/Code/condran.github.com/_posts/2013-03-24-how-to-use-ONGL-for-Java-introspection.md`
- **Domain config:** `/Users/paul/Developer/Code/condran.github.com/CNAME`
- **Profile photo:** `/Users/paul/Developer/Code/condran.github.com/img/instagram_me.jpg`

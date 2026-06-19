# Astro Conversion Todo
**Site:** paulcondran.com
**Plan ref:** `~/.claude/plans/dynamic-seeking-marble.md`
**Design ref:** `specimen.html` (in this repo)
**Target dir:** `~/Developer/Code/paulcondran.com/`

Mark items `[x]` as you complete them. Each phase should be fully done before moving to the next.

---

## Phase 1 — Scaffold

- [x] Run `npm create astro@latest paulcondran.com -- --template minimal` in `~/Developer/Code/`
- [x] `cd paulcondran.com && npm install`
- [x] `npm install @astrojs/rss @astrojs/sitemap`
- [x] Create `.gitignore` (node_modules, dist, .astro, .DS_Store, .idea)
- [x] `git init && git add -A && git commit -m "Initial Astro scaffold"`
- [x] `mkdir -p public/img`
- [x] Copy `condran.github.com/img/instagram_me.jpg` → `public/img/instagram_me.jpg`
- [x] Copy `condran.github.com/img/zombiehead.png` → `public/img/zombiehead.png`
- [ ] Copy `condran.github.com/img/favicon.ico` → `public/favicon.ico`
- [x] Create `public/robots.txt`:
  ```
  User-agent: *
  Allow: /
  Sitemap: https://paulcondran.com/sitemap-index.xml
  ```
- [x] Create `public/_headers` (Cloudflare Pages security headers):
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
- [x] Create `public/_redirects` (placeholder for future URL migrations — Cloudflare Pages):
  ```
  # /blog/* /writing/:splat 301
  ```
- [x] ~~No `public/CNAME` needed~~ — Cloudflare Pages uses dashboard for custom domain binding

---

## Phase 2 — Configuration

- [x] Write `astro.config.mjs`:
  - `site: 'https://paulcondran.com'`
  - `output: 'static'`
  - `build.format: 'directory'`
  - Shiki: `themes: { light: 'github-light', dark: 'github-dark' }`, `wrap: true`
  - Integrations: `sitemap()`
- [x] Write `src/content/config.ts` with `writing` collection schema:
  - Required: `title` (string), `summary` (string), `date` (coerce.date), `category` (enum: technology | creative-writing | philosophy)
  - Optional: `updated` (coerce.date), `tags` (string array, default []), `image` (string), `draft` (boolean, default false)
- [x] Verify TypeScript config extends `astro/tsconfigs/strict`

---

## Phase 3 — Styles

All styles extracted from `specimen.html`. Open that file alongside as reference.

### 3a — `src/styles/global.css`
- [x] CSS reset (`*, *::before, *::after`)
- [x] `:root` design tokens — copy from specimen lines 13–30:
  - `--bg`, `--bg-warm`, `--text`, `--text-secondary`, `--text-tertiary`
  - `--accent`, `--accent-light`, `--rule`, `--rule-light`, `--code-bg`
  - Font vars: `--font-display`, `--font-body`, `--font-ui`, `--font-accent`, `--font-code`
  - Layout vars: `--max-width: 900px`, `--content-width: 680px`, `--side-padding: 2rem`
- [x] `[data-theme="dark"]` overrides — copy from specimen lines 32–43
- [x] `html` base styles (font-size: 18px, scroll-behavior: smooth)
- [x] `body` base styles — copy from specimen lines 47–56 (including `transition` for dark mode)
- [x] Base element styles for Markdown content:
  - `a` — accent color, underline offset
  - `p` — margin between paragraphs
  - `h1`–`h4` — Instrument Serif, size scale
  - `strong`, `em`
  - `ul`, `ol` — indent, list-style
  - `blockquote` — border-left rule + padding
  - `hr` — 1px rule using `var(--rule)`
  - `img` — max-width: 100%, height: auto
  - `code` (inline) — JetBrains Mono, code-bg background, padding
  - `pre > code` — handled by Shiki + article.css
- [x] `@media (max-width: 700px)` — font-size: 16px, `--side-padding: 1.5rem`

### 3b — `src/styles/utilities.css`
- [x] `.container` — `max-width: var(--max-width); margin: 0 auto; padding: 0 var(--side-padding)`
- [x] `.content-width` — `max-width: var(--content-width)`
- [x] `.sr-only` — visually hidden, accessible to screen readers
- [x] `@keyframes fadeUp` — copy from specimen lines 540–542
- [x] Transition helpers — copy from specimen lines 122–132 (adapted for real class names)
- [x] `@media (prefers-reduced-motion: no-preference)` wrapper for animations

### 3c — `src/styles/components.css`
- [x] Theme toggle — copy from specimen lines 58–120 (`.theme-toggle`, `__track`, `__thumb`, `__label`)
- [x] Site header (`.site-header`):
  - Flex layout, space-between, sticky or static
  - Site title: Josefin Sans Light, letter-spacing, uppercase (adapt from specimen `.hierarchy__site-title`)
  - Nav links: Instrument Sans, small, letter-spacing
  - Active link state
- [x] Site footer (`.site-footer`) — adapt from specimen `.page-footer` (lines 558–570)
- [x] Post card (`.post-card`) — adapt from specimen `.card` styles (lines 396–481):
  - Category label
  - Title (Instrument Serif, 2.2rem)
  - Meta (Instrument Sans, tertiary color)
  - Summary (Newsreader, body size)
- [x] Category tag (`.category-tag`) — adapt from specimen `.hierarchy__tag` (lines 344–354)
- [x] Meta separator (`.meta-sep`) — 3px dot, adapt from specimen lines 336–342
- [x] Topic nav (`.topic-nav`) — flex row, small Instrument Sans links, active state underline
- [x] Post list (`.post-list`) — vertical stack with hairline dividers between cards
- [x] Responsive overrides at 700px

### 3d — `src/styles/article.css`
- [x] Article header (`.article__header`) — padding, border-bottom rule
- [x] Article title (`.article__title`) — Instrument Serif 3.2rem, adapt from specimen lines 306–313
- [x] Article subtitle (`.article__subtitle`) — Instrument Serif italic 1.35rem, adapt from specimen lines 315–321
- [x] Article meta bar (`.article__meta`) — flex, Instrument Sans 0.75rem, adapt from specimen lines 323–334
- [x] Article body (`.article__body`) — Newsreader 1.1rem, line-height 1.7, max-width `var(--content-width)`
- [x] Drop cap (`.article__body--drop-cap::first-letter`) — Instrument Serif, 3.6em float, adapt from specimen lines 364–372
- [x] Code blocks (`pre.astro-code`) — background `var(--code-bg)`, padding, border-radius, overflow-x auto
- [x] Shiki dark mode override:
  ```css
  [data-theme="dark"] pre.astro-code,
  [data-theme="dark"] pre.astro-code span {
    color: var(--shiki-dark) !important;
    background-color: var(--code-bg) !important;
  }
  ```
- [x] Pull quote (`.pull-quote`) — Instrument Serif italic 1.5rem, border-left accent, adapt from specimen lines 469–479
- [x] Caption (`.article__caption`) — Instrument Sans 0.72rem, tertiary, border-left rule, adapt from specimen lines 385–393
- [x] Post nav (`.post-nav`) — two-column grid, prev/next links with label + title
- [x] Responsive overrides at 700px (title to 2.2rem, etc.)

---

## Phase 4 — Components

### `src/components/BaseHead.astro`
- [x] Props: `title`, `description` (default), `image` (default), `canonicalURL`, `type`, `publishDate`
- [x] `<meta charset>`, `<meta viewport>`, `<title>`, `<meta description>`, `<link canonical>`
- [x] `<link rel="alternate">` for RSS feed
- [x] `<link rel="sitemap">`
- [x] Google Fonts `<link>` — copy exact URL from specimen.html line 9
- [x] Open Graph tags (`og:title`, `og:description`, `og:type`, `og:url`, `og:image`, `og:site_name`, `article:published_time`)
- [x] Twitter Card tags
- [x] Flash-prevention `<script is:inline>` — copy from specimen lines 545–548, adapted for Astro

### `src/components/ThemeToggle.astro`
- [x] HTML — copy from specimen lines 552–557 (`.theme-toggle` div with track, thumb, label)
- [x] `<script>` — copy full logic from specimen lines 809–851, convert to TypeScript-safe
  - `getPreferred()` — localStorage → system preference
  - `setTheme(theme)` — setAttribute, label text, localStorage
  - Click handler
  - Keyboard handler (Enter/Space)
  - `matchMedia` change listener
- [x] Scoped `<style>` or relies on `components.css` (keep consistent — use global CSS)

### `src/components/Header.astro`
- [x] Import and use correct Astro.url.pathname for active state
- [x] Site title link → `/`
- [x] Nav: Writing `/writing/`, About `/about/`, Colophon `/colophon/`
- [x] `class:list` active state logic
- [x] Semantic `<header>` + `<nav aria-label="Main">`

### `src/components/Footer.astro`
- [x] Dynamic year via `new Date().getFullYear()`
- [x] Links: Colophon, RSS feed
- [x] Semantic `<footer>`

### `src/components/PostCard.astro`
- [x] Props: `title`, `summary`, `date`, `category`, `slug`, `readingTime?`
- [x] Category label mapping (technology → "Technology", creative-writing → "Creative Writing", philosophy → "Philosophy")
- [x] Formatted date (e.g. "20 March 2013" — `en-AU` locale)
- [x] Title as `<a>` link to `/writing/[slug]/`
- [x] Conditional reading time display
- [x] Semantic `<article>`

### `src/components/PostMeta.astro`
- [x] Props: `date`, `category`, `readingTime`
- [x] Formatted date with `<time datetime={iso}>` for accessibility
- [x] `.meta-sep` dots between items
- [x] `.category-tag` span

### `src/components/PostNav.astro`
- [x] Props: `prev?`, `next?` (each: `{ slug, title }`)
- [x] Two-column grid, left = prev, right = next
- [x] Show only what exists (conditional rendering)
- [x] "Previous" / "Next" labels + post title as link

---

## Phase 5 — Layouts

### `src/layouts/BaseLayout.astro`
- [x] Import: BaseHead, Header, Footer, ThemeToggle
- [x] Import all 4 CSS files (global, utilities, components, article)
- [x] Props forwarded to BaseHead
- [x] Structure: `<!DOCTYPE html>` → `<html lang="en" data-theme>` → `<head>` → `<body>` → ThemeToggle + Header + `<main><slot/></main>` + Footer
- [x] `data-theme` attribute not needed on HTML here — the inline script in BaseHead sets it on `document.documentElement` at runtime

### `src/layouts/PostLayout.astro`
- [x] Import: BaseLayout, PostMeta, PostNav
- [x] Import: `article.css` (if not already included via BaseLayout)
- [x] Props: `title`, `summary`, `date`, `category`, `readingTime`, `prev?`, `next?`
- [x] Title passed to BaseLayout as `${title} — Paul Condran`
- [x] `type="article"` and `publishDate` passed to BaseLayout → BaseHead
- [x] Article structure: `.article__header` (h1 title + subtitle + PostMeta) → `.article__body.article__body--drop-cap` `<slot/>` → PostNav

---

## Phase 6 — Pages

### `src/pages/index.astro` (Homepage)
- [x] `getCollection('writing', draft filter)` + sort newest-first + slice(0, 6)
- [x] `getReadingTime(body)` helper (words / 200, Math.ceil)
- [x] Hero section: large Josefin Sans site title + Newsreader tagline
- [x] Section label "Recent Writing" (Instrument Sans, uppercase, `--accent`)
- [x] Map to `<PostCard>` components
- [x] "View all writing →" link to `/writing/`

### `src/pages/writing/index.astro` (All Writing)
- [x] `getCollection` + sort newest-first (no slice)
- [x] Page header: title "Writing" + subtitle
- [x] Topic nav: All (active) / Technology / Creative Writing / Philosophy
- [x] Map all posts to `<PostCard>` components

### `src/pages/writing/technology.astro`
- [x] Filter collection: `data.category === 'technology'`
- [x] Topic nav with Technology active
- [x] Page header: "Technology"
- [x] Map filtered posts to `<PostCard>`

### `src/pages/writing/creative-writing.astro`
- [x] Same pattern, filter `'creative-writing'`, label "Creative Writing"

### `src/pages/writing/philosophy.astro`
- [x] Same pattern, filter `'philosophy'`, label "Philosophy"

### `src/pages/writing/[...slug].astro` (Individual Post)
- [x] `getStaticPaths()` — all non-draft posts, sorted chronologically, with prev/next props
- [x] `render(post)` to get `<Content />`
- [x] `getReadingTime(post.body)`
- [x] Pass all props to `<PostLayout>`
- [x] Render `<Content />`

### `src/pages/about.astro`
- [x] BaseLayout with title "About — Paul Condran"
- [x] Profile photo `public/img/instagram_me.jpg`
- [x] Bio content (update from old "Java programmer by day" to current, genuine bio)
- [x] Interests: technology, writing, philosophy, whisky 🥃

### `src/pages/colophon.astro`
- [x] BaseLayout with title "Colophon — Paul Condran"
- [x] Sections: Type System / Technology / Design Philosophy
  - Type System: describe the 5 fonts and their roles (reference specimen.html)
  - Technology: Astro, Cloudflare Pages, Google Fonts, no JS frameworks, custom CSS
  - Design: editorial minimalism, content-first, warm palette rationale

---

## Phase 7 — Content & Feeds

### Migrate posts
- [x] Create `src/content/writing/github-game-off-lessons.md`
  - New frontmatter: `title`, `summary`, `date: 2013-03-20`, `category: technology`, `tags: ["game-development", "javascript", "cocos2d"]`, `image: /img/zombiehead.png`
  - Body: copy unchanged from `condran.github.com/_posts/2013-03-20-what-i-learnt-doing-github-game-off.md`
- [x] Create `src/content/writing/java-ognl-introspection.md`
  - New frontmatter: `title`, `summary`, `date: 2013-03-24`, `category: technology`, `tags: ["java", "ognl", "reflection"]`
  - Body: copy unchanged from `condran.github.com/_posts/2013-03-24-how-to-use-ONGL-for-Java-introspection.md`
- [x] Verify both posts pass Zod schema validation (run `npm run dev`, check terminal for errors)

### RSS Feed
- [x] Create `src/pages/feed.xml.ts`
  - `GET()` function using `@astrojs/rss`
  - All non-draft posts sorted newest-first
  - Items: `title`, `pubDate`, `description` (summary), `link`, `categories`
  - `customData: '<language>en-au</language>'`

---

## Phase 8 — Deployment (Cloudflare Pages)

> No GitHub Actions workflow needed — Cloudflare Pages has built-in CI that connects directly to your repo.

### 8a — Push to GitHub
- [ ] Create new GitHub repository (suggested name: `paulcondran.com`)
- [ ] `git remote add origin git@github.com:condran/paulcondran.com.git`
- [ ] `git push -u origin main`

### 8b — Connect to Cloudflare Pages
- [ ] Log in to [dash.cloudflare.com](https://dash.cloudflare.com)
- [ ] Workers & Pages → Create → Pages → Connect to Git
- [ ] Authorise GitHub, select the `paulcondran.com` repo
- [ ] Build settings:
  - Framework preset: **Astro** (auto-detected)
  - Build command: `npm run build`
  - Build output directory: `dist`
- [ ] Click Save and Deploy — first build runs automatically
- [ ] Verify build completes green at `<project>.pages.dev` ✅

### 8c — Custom domain
- [ ] In Cloudflare Pages project → Custom domains → Add custom domain
- [ ] Enter `paulcondran.com`
- [ ] Follow Cloudflare's DNS prompts (adds CNAME automatically if domain is on Cloudflare DNS, or gives you the record to add manually)
- [ ] Wait for DNS propagation + automatic HTTPS provisioning
- [ ] Verify `https://paulcondran.com` loads ✅

### 8d — Enable Web Analytics (optional but recommended)
- [ ] In Cloudflare Pages project → Settings → Web Analytics → Enable
- [ ] Zero-JS, privacy-respecting — no code changes needed, Cloudflare injects the beacon automatically

### 8e — Verify automatic deploys
- [ ] Make a small commit and push to `main`
- [ ] Confirm Cloudflare Pages triggers a new build automatically
- [ ] Confirm preview deployments work on feature branches (each push to a non-main branch gets its own `<branch>.<project>.pages.dev` URL)

---

## Phase 9 — Verification

### Dev server checks
- [ ] `npm run dev` — no console errors at startup
- [ ] All routes resolve without 404:
  - [ ] `/` — homepage
  - [ ] `/writing/` — all posts
  - [ ] `/writing/technology/` — filtered
  - [ ] `/writing/creative-writing/` — filtered (0 posts is fine, shows empty state)
  - [ ] `/writing/philosophy/` — filtered (0 posts is fine)
  - [ ] `/writing/github-game-off-lessons/` — full post renders
  - [ ] `/writing/java-ognl-introspection/` — Java code blocks syntax-highlighted
  - [ ] `/about/` — photo loads, bio content
  - [ ] `/colophon/` — content renders
  - [ ] `/feed.xml` — valid RSS XML

### Typography checks (compare against specimen.html)
- [ ] Site title: Josefin Sans Light, spaced, uppercase
- [ ] Nav links: Instrument Sans
- [ ] Post titles (cards): Instrument Serif ~2.2rem
- [ ] Article title: Instrument Serif ~3.2rem
- [ ] Article subtitle: Instrument Serif italic
- [ ] Body text: Newsreader 1.1rem, line-height ~1.7
- [ ] Metadata, tags, dates: Instrument Sans small
- [ ] Code blocks: JetBrains Mono
- [ ] Drop cap on first paragraph of posts

### Dark mode checks
- [ ] Toggle switches theme correctly
- [ ] Theme persists on page refresh (no flash)
- [ ] System preference respected on first visit (clear localStorage to test)
- [ ] Code blocks change between light/dark Shiki themes
- [ ] All text/background/border colors transition smoothly (0.4s)

### Responsive checks (resize to 700px)
- [ ] Font size reduces (18px → 16px)
- [ ] Side padding reduces (2rem → 1.5rem)
- [ ] Post card layout stacks
- [ ] Nav doesn't overflow (consider hamburger or wrapping if needed)
- [ ] Article title reduces to ~2.2rem

### Build checks
- [ ] `npm run build` — no errors, no type errors
- [ ] `npx serve dist` — test production build locally
- [ ] `dist/_headers` exists (Cloudflare security headers file)
- [ ] `dist/sitemap-index.xml` generated
- [ ] `dist/feed.xml` generated and valid XML
- [ ] No broken internal links
- [ ] Confirm no `dist/CNAME` file (not needed for Cloudflare Pages)

### Lighthouse (run on production build via `npx serve dist`)
- [ ] Performance: 95+
- [ ] Accessibility: 100
- [ ] Best Practices: 100
- [ ] SEO: 100

---

## Post-Launch

- [ ] Write first new post (Technology, Creative Writing, or Philosophy)
- [ ] Add `og-default.png` social share image to `public/img/`
- [ ] Consider self-hosting fonts via Fontsource for privacy (removes Google Fonts dependency)
- [ ] Archive `condran.github.com` repo — add note to README pointing to new site
- [ ] Update Twitter/GitHub profile links if needed
- [ ] Check Cloudflare Web Analytics dashboard after a few days — zero-JS, no cookies, GDPR-friendly
- [ ] Review Cloudflare Pages → Settings → Builds & Deployments — confirm branch deploy previews are active

---

## Quick Reference

| Token | Light | Dark |
|-------|-------|------|
| `--bg` | #FAFAF8 | #161514 |
| `--text` | #1a1a1a | #e8e4df |
| `--text-secondary` | #6b6560 | #9e9590 |
| `--text-tertiary` | #9e9891 | #6b6460 |
| `--accent` | #c4653a | #d4825e |
| `--rule` | #ddd8d0 | #2e2b28 |
| `--code-bg` | #f0ede8 | #1f1d1b |

| Font | Role | Weight |
|------|------|--------|
| Instrument Serif | Display / Headlines | Regular, Italic |
| Newsreader | Body / Long-form | 300–700 + Italic |
| Instrument Sans | UI / Meta / Nav | 400, 500, 600 |
| Josefin Sans | Site title accent | 300 (Light) |
| JetBrains Mono | Code | 400, 500 |

| Breakpoint | Change |
|-----------|--------|
| ≤ 700px | font-size: 16px, --side-padding: 1.5rem |

**Reading time formula:** `Math.ceil(wordCount / 200)` words per minute

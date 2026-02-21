# marini.jp-astro

Astro rewrite of marini.jp — a Japanese indie publishing company site (English-learning fiction).

## Origin

This project is a migration from a **Statamic 5** (Laravel/PHP) flat-file CMS site. The original source lives at `../marini.jp`. Refer to it when migrating content or checking how something was implemented.

## Tech Stack

- **Astro 5** (static site generation)
- **Tailwind CSS v4** via `@tailwindcss/vite` (NOT v3 — no `tailwind.config.js`)
- **TypeScript** (strict mode)
- **Riyo** for content management (schemas in `riyo.content.*.yaml`)
- No frontend framework (no React/Vue/Svelte) — plain Astro components + vanilla JS

## Tailwind v4 Notes

Tailwind v4 uses a CSS-first configuration. All customization lives in `src/styles/global.css`:
- `@theme { }` — custom colors, fonts, shadows, etc. (replaces `tailwind.config.js` `theme.extend`)
- `@plugin "@tailwindcss/typography"` — plugin loading (replaces `require()` in config)
- Single `@import "tailwindcss"` — replaces the three separate base/components/utilities imports

## Project Structure

```
src/
├── components/        # Reusable Astro components (BannerSlider, PublicationGrid, UpdateItem)
├── content/           # Content collections (markdown files)
│   ├── banners/
│   ├── contributors/
│   ├── publications/
│   ├── series/
│   └── updates/
├── data/              # Site-wide data (nav, globals — replaces Statamic globals)
├── layouts/           # Layout.astro (base HTML shell)
├── pages/             # File-based routing
├── styles/            # global.css (Tailwind + fonts + theme)
└── content.config.ts  # Zod schemas for all content collections
public/
├── branding/          # Logo variants
├── fonts/             # Self-hosted woff2 fonts (Murecho, NotoSerif JP)
├── icons/             # Update icons
└── *.jpg/png          # Cover images, banners
```

## URL Structure

| URL | Page |
|-----|------|
| `/` | Home page |
| `/publications/{slug}` | Publication detail |
| `/series/{slug}` | Series detail |
| `/contributors/{slug}` | Contributor detail |

## Statamic → Astro Concept Cheatsheet

| Statamic | Astro |
|---|---|
| `layout.antlers.html` | `src/layouts/Layout.astro` with `<slot />` |
| `{{ partial:ui/thing }}` | `import Thing from "../components/Thing.astro"` then `<Thing />` |
| `{{ collection from="x" sort="..." }}` | `getCollection("x")` + `.sort()` in frontmatter |
| `content/globals/*.yaml` | `src/data/globals.ts` (plain TS exports) |
| Bard field (ProseMirror JSON) | Markdown body in `.md` files |
| `{{ template_content }}` | `<slot />` |

## Commands

```sh
npm run dev      # Start dev server
npm run build    # Build static site to dist/
npm run preview  # Preview the built site
```

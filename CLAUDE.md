# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file static personal portfolio site for Tom Papazoglou (tpapazoglou.me). No build tools, no package manager, no JavaScript, no external CSS file. Everything — HTML structure, all CSS, and all content — lives in `index.html`. `avatar.jpg` is the profile photo, but it's referenced via a GitHub raw URL in the Open Graph/Twitter meta tags rather than a local relative path.

## Development

There is no build step. To preview locally, open `index.html` directly in a browser or serve it with any static file server:

```bash
python3 -m http.server 8080
```

To deploy, commit and push — the site is served from the `main` branch via GitHub Pages or equivalent static hosting.

## CSS architecture

All styles are in an inline `<style>` block in `<head>`. The design system is defined via CSS custom properties in `:root`:

- `--bg` / `--surface` / `--surface-2` — background levels
- `--ink` / `--ink-mid` / `--ink-faint` — text hierarchy
- `--green` / `--green-dark` / `--green-light` / `--green-border` — accent palette
- `--sans` → `Outfit` (weights 300/400/500/700/800), `--mono` → `IBM Plex Mono` (weights 400/500)

Always use these variables rather than raw colour values. Both fonts are loaded from Google Fonts.

## Layout

The page shell is a two-column CSS grid: a `64px` sticky `.rail` (left navigation) and a `.main` content area. The responsive breakpoint is `860px` — below it the rail collapses to a top bar and multi-column grids drop to single column.

Sections follow a consistent pattern: a `.section-meta` row containing a monospace section number and an `h2`, then section-specific content grids.

## Content structure

Five sections numbered sequentially in the HTML:
1. `#hero` — large typographic header + five stat cards + ticker bar
2. `#about` (01) — two-column prose
3. `#journey` (02) — numbered `.journey-item` rows using a three-column grid
4. `#thinking` (03) — `.thoughts-grid` of `.thought-card` variants (standard, `thought-featured`, `thought-dark`, `thought-network`, `thought-genx`)
5. `#writing` (04) — `.writing-grid` of `.writing-card` items + a full-width `.writing-cta`
6. `#contact` (05) — five `.contact-card` links in a five-column grid

All writing cards currently show "Coming soon". They are authored in a separate Claude project and published to Medium. Each card has a `data-post` slug for easy targeting.

### Publishing a post

When a post goes live on Medium, give a Claude Code session the Medium URL and the post slug (or title). The changes needed are:

1. Find the card by its `data-post` attribute, e.g. `data-post="building-teams-like-an-intrapreneur"`
2. Change the element from `<div class="writing-card" ...>` to `<a class="writing-card" ... href="[medium-url]" target="_blank" rel="noopener">`  
   — the CSS already handles `<a>` elements correctly (no underline, correct colour)
3. Update `.wc-label` from `Article · Coming soon` to `Article · [Mon YYYY]`

The six post slugs are:
- `leading-with-and-without-authority`
- `building-teams-like-an-intrapreneur`
- `the-bottleneck-is-never-the-technology`
- `building-my-personal-ai-operating-system`
- `human-in-the-loop`
- `why-tokenomics-matter`

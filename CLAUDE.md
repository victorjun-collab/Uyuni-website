# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development

```bash
npx serve -l 3000 .
```

No package.json, no build step. Static HTML served directly.

## Architecture

Uyuni company website with two business sectors, each with its own page and color theme:

```
index.html          — Landing hub: dual-panel selector (Logistics / Hotels)
├── logistics.html  — Logistics Real Estate sector (standalone page)
└── hotels.html     — Hotels & Branded Properties sector (standalone page)
```

**index.html has dual behavior:** It serves as both a landing page AND contains embedded content for both sectors (toggled via JS `enterSector()`/`showLanding()`). The standalone pages (`logistics.html`, `hotels.html`) duplicate their sector's content for direct URL access.

### Styling approach

- All CSS is inline in each HTML's `<style>` block — no shared base CSS file
- `responsive.css` is the only external CSS, shared by all 3 pages (media queries + hamburger menu)
- CSS custom properties defined identically in all 3 files (`:root` block)
- Fonts loaded via Google Fonts CDN: Cormorant Garamond (serif headings), DM Sans (body), DM Mono (labels/mono)

### Color themes

- **Logistics:** Navy (`--navy:#0d1b2a`), blue accents (`--blue:#2a5fa5`)
- **Hotels:** Warm dark (`--hotel-bg:#1a0f08`), gold accents (`--gold:#c9a96e`)
- Hotels pages use `body.hotel-page` class for overlay theme switching

### Section IDs

Logistics uses `#hero`, `#intro`, `#services`, `#why`, `#track-record`, `#team`, `#recognition`, `#contact`.
Hotels prefixes with `h-`: `#h-hero`, `#h-intro`, `#h-services`, `#h-why`, `#h-portfolio`, `#h-team`, `#h-contact`.

## Critical: Base64 images

Background images and logos are base64-encoded inline. **Do not bulk-replace base64 strings.**

- CSS `url('data:image/png;base64,...')` → background image (JPEG stored as PNG data URI, starts with `/9j/`)
- `<img src="data:image/png;base64,...">` → logo (real PNG, starts with `iVBOR`)

Files are 500–960 lines but most content is on ~6 base64 lines. Read with offset/limit to skip them.

## Responsive breakpoints (responsive.css)

- **≤1024px**: Hamburger menu replaces desktop nav links, grids collapse to fewer columns
- **≤768px**: Single-column layouts, reduced padding, scroll indicator hidden
- **≤480px**: Minimal padding, smaller typography

Mobile menu: `.hamburger` button toggles `.nav-overlay` fullscreen menu. JS functions `toggleMobileMenu()`/`closeMobileMenu()` in each file's inline script.

## When editing content

Content changes must be applied to **both** `index.html` (embedded sector) **and** the standalone page (`logistics.html` or `hotels.html`) to stay in sync. CSS layout changes go in `responsive.css` for mobile or in each file's `<style>` for desktop.

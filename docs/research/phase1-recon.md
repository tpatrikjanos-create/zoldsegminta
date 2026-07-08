# Phase 1 Reconnaissance — kifli.hu

## Environment note

Chromium/Playwright could not tunnel through this session's agent proxy
(`ERR_CONNECTION_RESET` on every HTTPS host, including `example.com`), while
plain HTTP clients (`curl`, Python `urllib`) worked fine through the same
proxy. Root cause not resolved — likely a Chromium-specific TLS/CONNECT
behavior the proxy rejects. As a result, this phase was done via raw
HTML/CSS fetch (no screenshots, no `getComputedStyle`, no interaction
sweep). Phase 1 should be redone with real browser screenshots once
Playwright can reach the internet in this environment.

## Site basics

- URL: https://www.kifli.hu/
- Stack: Next.js (SSR), CSS Modules with hashed classnames, `next/font`
- Title: `Kifli.hu | Online szupermarket - Minőségi online bevásárlás`
- Theme attribute: `data-theme="avocado-light"` (dark variant `avocado-dark`
  also present in the CSS — tokens are duplicated per theme)
- Main CSS bundles: `/_next/static/css/{065642c2b0458ad1,714358d7680e847c,84bfee8029d8a1b1,a36706922c0172fa}.css`

## Typography

- Primary font: **Inter**, self-hosted via `next/font` (`font-display:optional`)
- Fallback stack: `-apple-system, BlinkMacSystemFont, "San Francisco", "Segoe UI", "Helvetica Neue", "Liberation Sans", Roboto, sans-serif`
- Font-size scale (tokens): 10, 12, 13, 14, 16, 18, 20, 22, 24, 36, 48px
  (`--font-size-60` … `--font-size-800`)

## Color tokens (light theme)

Brand green (primary), light → dark:
```
--green-10:  #f1faf2
--green-20:  #ddf4de
--green-30:  #c6ebc8
--green-40:  #aee3b1
--green-50:  #8fcd95
--green-60:  #9dcb9a   (also read as #2f7d3b in some contexts — check per-usage)
--green-70:  #b9e1b6
--green-80:  #cdeaca
--green-90:  #e1f4df
--green-100: #f3faf3
```
Note: green tokens are redefined between the two theme blocks (light vs dark
palette share the same variable names with swapped values) — re-verify exact
hex per use once we can screenshot the live site.

Blue (secondary/info):
```
--blue-10..100: #f3f7f9 → #07171f (same light/dark swap pattern as green)
```

Neutrals / brand:
```
--color-header: #252525 / #f2f4f4
--color-header-dark: #1a1a1a / #dfded7
--color-discount-yellow: #ffe55a
--color-like: #ff455b
--color-brand-facebook: #3b5998
--color-brand-twitter: #00acee
--color-brand-pharmacy: #003f7c
--background-color-white: #fff
--background-color-image-overlay: #d8d8d8 / #f6f6f6
```
Alpha scales: `--alpha-{5,10,20,30,40,50,60,70,80,90,100}` (black-based) and
`--alphaWhite-{...}` (white-based), standard 10-step opacity ramps.

## Spacing / size scale

`--size-*` tokens (px): 1, 2, 4, 8, 12, 16, 20, 24, 28, 32, 40, 44, 48, 56, 60,
64, 72, 80, 120, 320.

## Breakpoints

```
--breakpoint-2xs: 361px
--breakpoint-xs:  567px
--breakpoint-sm:  768px
--breakpoint-md:  960px
--breakpoint-ml:  1024px
--breakpoint-lg:  1260px
--breakpoint-xl:  1680px
--breakpoint-2xl: 1761px
--breakpoint-3xl: 1921px
```

## Open items for a real browser pass (Phase 1 continuation)

- [ ] Full-page screenshots at 1440 / 768 / 390px
- [ ] Favicon URL
- [ ] Exact resolved brand green/blue hex actually rendered (light theme)
- [ ] Header/nav structure, hero section, product card layout
- [ ] Interaction sweep: scroll-driven effects, hover states, click handlers,
      any modal/tab components
- [ ] Confirm smooth-scroll library usage (Lenis/Locomotive) — none detected
      in static HTML/CSS so far
- [ ] Border-radius scale actual px values (currently only see `var()`
      references, not resolved)
- [x] Favicon downloaded (`assets/favicon/favicon.ico`, `favicon-32x32.png`)

## Phase 2 — Foundation Build (done, screenshot-less)

- `assets/tokens.css` — color/typography/spacing/breakpoint tokens above,
  wired into `index.html` via `<link rel="stylesheet">`.
- `assets/icons/` — hero + app-badge SVGs pulled from `cdn.kifli.hu`
  (`hero-award.svg`, `hero-fresh.svg`, `hero-products.svg`, `star.svg`).
- `assets/favicon/` — `favicon.ico`, `favicon-32x32.png`.
- Not done (needs a working browser): SVG icon *inventory* per component,
  TypeScript interfaces (n/a — this repo is static HTML, no build step),
  `npm run build` verification (n/a for the same reason).

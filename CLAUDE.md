# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static game portal ("Neon Arcade") — a single `index.html` that lists browser games and links out to them. No build step, no dependencies, no framework.

## Running locally

```bash
open index.html
# or serve with any static server:
python3 -m http.server 8080
```

## Architecture

The entire site is one file: `index.html` with embedded CSS.

**Layout structure:**
- `<header>` — "NEON ARCADE" logo + tagline
- `<main>` — CSS grid of `.card` elements, one per game
- `<footer>` — copyright line

**Theming system:**
Each card carries a `data-game` attribute that sets a `--card-accent` CSS variable. All card colours (border glow, title, tags, button) derive from that single variable — adding a new game only requires adding a new `[data-game="x"] { --card-accent: ... }` rule.

**Visual effects** are pure CSS: scanline overlay (`body::after`), scrolling grid in preview areas (`::before` with `background-image`), and `@keyframes pulse`/`flicker`/`gridscroll`.

## Adding a new game

1. Add a new `<article class="card" data-game="SLUG">` block inside `<main>`, following the existing card structure.
2. Set the accent colour in CSS: `.card[data-game="SLUG"] { --card-accent: #???; }`.
3. Point the `.play-btn` `href` to the game's hosted URL.

## Game repos (separate GitHub repos, not submodules)

| Game | Repo | Expected deploy URL |
|------|------|---------------------|
| Neon Breach FPS | `kevinm2k/neon-breach-fps` | `https://kevinm2k.github.io/neon-breach-fps/` |
| Neon Driver     | `kevinm2k/neon-driver`     | `https://kevinm2k.github.io/neon-driver/`     |

Both games are zero-dependency vanilla JS + HTML5 Canvas. They live in sibling directories (`../neon-breach-fps`, `../neon-driver`) but are deployed independently. This portal simply links to their deployed URLs — it does not embed or bundle them.

## Deployment

Deploy `index.html` to any static host (GitHub Pages, Netlify, Vercel, etc.). No build output directory — the file itself is the artifact.

For GitHub Pages: push to `main`, set Pages source to root `/` or use a `gh-pages` branch.

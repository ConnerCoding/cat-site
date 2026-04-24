# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

A single-page static website ("Cat Site") — the author's first web project. There is no build system, package manager, test runner, or linter. The entire site is three files plus images.

- `index.html` — the only page, structured as three sections (`#who`, `#what`, `#where`) inside a Bootstrap 4 navbar layout.
- `styles/styles.css` — site-specific overrides layered on top of Bootstrap.
- `images/` — JPGs referenced by `index.html`; `project.png` is used only by the README.

## Running locally

No tooling is configured. To view changes, open `index.html` directly in a browser, or serve the directory (e.g. `python3 -m http.server`) so relative paths under `styles/` and `images/` resolve.

## Architecture notes

- **Design system is neobrutalist, layered on top of Bootstrap 4.** Bootstrap 4.3.1 CSS, jQuery 3.5.1, Popper 1.16.0, Bootstrap 4.4.1 JS, Font Awesome 4.7.0, and the Bungee + Space Grotesk Google Fonts load from CDNs in `index.html`. Bootstrap provides the grid and navbar skeleton; `styles/styles.css` owns nearly all of the visual design and overrides most Bootstrap defaults (`.btn`, `.img-thumbnail`, `.nav-link`). Prefer Bootstrap classes for layout (`container-fluid`, `row`, `col-sm-4`, `navbar-*`, `ml-auto`) and everything else in `styles.css`.
- **Palette and shadow tokens live in `:root`** as CSS custom properties: colors (`--c-cream`, `--c-ink`, `--c-pink`, `--c-mint`, `--c-butter`, `--c-lilac`, `--c-paper`) and reusable `--border`, `--border-fat`, `--shadow`, `--shadow-lg`. The brutalist look is consistent because borders are always `3–4px solid var(--c-ink)` and shadows are hard offsets (`Npx Npx 0 var(--c-ink)`) — never `blur`. Add colors as new variables rather than hardcoding.
- **Every section has a distinct flat background color** (`#who` mint, `#what` pink, `#where` lilac, footer ink) plus a dotted-paper background texture on `body` and `.whoami`. Sections contain absolutely-positioned `.sticker` decorations (`.sticker--tl/tr/bl/br` + variant class) for corner ornaments — don't use these for meaningful content, they're `aria-hidden` on purpose.
- **`#who` is a full-viewport hero** (`min-height: 95vh`, flex-centered). The title is split into three `<span class="hero-title__line">` chunks that get individually rotated and shadowed; the cat portrait is wrapped in a `.polaroid` with a `wiggle` keyframes animation; the tagline is a rotated ink pill; a `.scroll-cue` chevron bobs at the bottom. If you add content inside `#who`, expect the flex column layout.
- **Nav is sticky with a hard bottom border** (no blur). `position: sticky; top: 0` lives in `styles.css`, not as Bootstrap's `sticky-top`. The `.navbar-brand` is styled like a pinned sticker (butter background, hard shadow, `rotate(-2deg)`). Sections use `scroll-margin-top: 90px` so anchor jumps clear the bar.
- **Between `#who` and `#what` is a `.meow-ticker` marquee.** It's three duplicated `<span>` children inside `.meow-track`; the CSS animation translates the track by `-33.333%`, so if you change the number of spans you must match the translate percentage (`-100% / count`).
- **`.photo-card` (in `#where`) replaces Bootstrap's thumbnail styling** with a polaroid-card look. Each card is given a specific tilt via `:nth-child` and straightens + lifts on hover. Color variants (`.photo-card--pink/--mint/--butter`) drive both the image background-behind-transparent-PNG and the `.tag` chip.
- **A small inline `<script>` at the end of `<body>` renders a paw-print cursor trail** on desktop. It bails out early if `pointer: coarse` (touch) or `prefers-reduced-motion: reduce` matches. New elements (`.cursor-paw`) are appended to `document.body` and removed after 1.1s. Keep any DOM that grows unboundedly off of `mousemove` similarly throttled.
- **Respect reduced-motion.** A global `@media (prefers-reduced-motion: reduce)` block in `styles.css` disables all animations and transitions — including the marquee — so don't add motion that only works via JS without an equivalent opt-out.
- **Only in-page anchors in the nav.** `#who`, `#what`, `#where` all resolve to real section `id`s. The site is a single page — don't add links to nonexistent routes.

## Git workflow for this environment

All development for Claude Code sessions here happens on the branch `claude/add-claude-documentation-0BPCO`. Create it locally if missing, commit with descriptive messages, and push with `git push -u origin <branch>`. Do not push to other branches without explicit permission, and do not open a PR unless asked.

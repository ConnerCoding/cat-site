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

- **Styling is Bootstrap 4 + a substantial custom sheet.** Bootstrap 4.3.1 CSS, jQuery 3.5.1, Popper 1.16.0, Bootstrap 4.4.1 JS, Font Awesome 4.7.0, and the Montserrat + Fredoka Google Fonts load from CDNs in `index.html`. `styles/styles.css` owns the visual design: CSS custom properties in `:root` define the palette (`--c-teal`, `--c-plum`, `--c-cream`, `--c-ink`, `--c-accent`), and section classes (`.whoami`, `.whatami`, `.whereami`) pull from them for gradient backgrounds, typography, hover transitions, and the sticky/blur navbar. Prefer Bootstrap classes for layout (`container-fluid`, `row`, `col-sm-4`, `navbar-*`, `ml-auto`) and add anything visual to `styles.css`. When introducing new color, add a CSS variable rather than hardcoding.
- **`.whoami` is a full-viewport hero** (`min-height: 90vh`, flex-centered) with a floating cat image animation — its layout differs from the other two sections, so if you add content inside `#who`, expect the flex column to stack it vertically and centered.
- **Nav is sticky with a blur backdrop.** `position: sticky; top: 0` plus `backdrop-filter: blur(12px)` lives in `styles.css`, not as a Bootstrap `sticky-top` class. Sections use `scroll-margin-top` so anchor jumps don't hide behind the bar.
- **Only in-page anchors in the nav.** `#who`, `#what`, `#where` all resolve to real section `id`s. The site is a single page — don't add links to nonexistent routes.

## Git workflow for this environment

All development for Claude Code sessions here happens on the branch `claude/add-claude-documentation-0BPCO`. Create it locally if missing, commit with descriptive messages, and push with `git push -u origin <branch>`. Do not push to other branches without explicit permission, and do not open a PR unless asked.

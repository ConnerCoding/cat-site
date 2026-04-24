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

- **Styling is Bootstrap 4 + a small custom sheet.** Bootstrap 4.3.1 CSS, jQuery 3.5.1, Popper 1.16.0, Bootstrap 4.4.1 JS, Font Awesome 4.7.0, and the Montserrat Google Font are all loaded from CDNs in `index.html`. `styles/styles.css` only overrides selectors for the three section classes (`.whoami`, `.whatami`, `.whereami`), the navbar, footer, and a few utility classes — prefer Bootstrap 4 classes for layout and reach for the custom sheet only for section-specific color/spacing.
- **Navbar links mix routing styles.** The `WHO` link points to `/who` (a nonexistent path), while `WHAT` and `WHERE` use in-page anchors (`#what`, `#where`). The matching `id="who"` exists on the first section, so if you touch the nav, be aware of the inconsistency rather than "fixing" it blindly.
- **Known small issues in `index.html`** worth preserving context on: an empty `<a href="">Empty</a>` inside `.whoami` (line 36), `containter-fluid` typo on the `.whatami` div (line 41), and the stray `r` after the closing `</a>` in the footer making the name render as "Conner Schiller" (lines 78–79). Don't silently reformat these unless the task asks — they may be intentional or part of the learning exercise.

## Git workflow for this environment

All development for Claude Code sessions here happens on the branch `claude/add-claude-documentation-0BPCO`. Create it locally if missing, commit with descriptive messages, and push with `git push -u origin <branch>`. Do not push to other branches without explicit permission, and do not open a PR unless asked.

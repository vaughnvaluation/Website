# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The marketing website for Vaughn Valuation, a residential real estate appraisal firm serving North Texas and the Texoma region. It is a static, framework-free HTML site with five pages: `index.html`, `about.html`, `services.html`, `faq.html`, `contact.html`.

## Development

There is no build system, package manager, or dependency list — each HTML file is fully self-contained (inline `<style>` and `<script>`, no external JS/CSS files besides Google Fonts). To work on the site, just open the HTML files directly in a browser or serve the directory with any static file server, e.g.:

```bash
python3 -m http.server 8000
```

There are no tests, linters, or build/deploy commands to run.

## Architecture

- **No shared layout or templating.** Each page duplicates the full `<style>` block, nav markup, footer markup, and mobile-menu script. There is no include mechanism (no server-side includes, no JS componentization) — when changing shared UI (nav, footer, color variables, fonts, button styles), the edit must be repeated identically across all five HTML files.
- **Design tokens** are CSS custom properties defined per-page at the top of each `<style>` block under `:root` (`--navy`, `--gold`, `--cream`, etc.). Keep these values in sync across pages.
- **Fonts**: Cormorant Garamond (serif, headings/display) and Outfit (sans-serif, body), loaded via a Google Fonts `<link>` in each page's `<head>`.
- **Favicon** is a real file, `favicon.png` (512×512), referenced via `<link rel="icon" href="favicon.png">` on every page — this is the one actual image file in the repo. It was previously an inlined base64 `data:image/png` URI, but Google Search doesn't reliably crawl favicons defined that way, so it was switched to a stable file URL. The **nav logo** is still inlined as a base64 `data:image/png` URI directly in the HTML.
- **Mobile nav** is a hamburger menu toggled by a small inline `<script>` (`toggleMenu()`), duplicated per page.
- **Contact form** (`contact.html`) submits client-side via `fetch` to [Web3Forms](https://web3forms.com) (`https://api.web3forms.com/submit`), using a public `access_key` embedded in the form's hidden input — there is no backend in this repo. Form submission and the success-message swap are handled entirely in an inline `<script>` at the bottom of the page.
- Internal navigation is plain relative links between the five HTML files (no router).

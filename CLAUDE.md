# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static portfolio website for KUEICHI (an embedded software engineer), hosted on Cloudflare Pages at `kueichi.dev`. No build system, no package manager — pure HTML, CSS, and vanilla JavaScript.

## Development

Open files directly in a browser. There is no dev server, build step, or dependency installation required.

To preview locally with live reload, any static file server works:

```bash
# Python (no install needed)
python -m http.server 8080

# Node (if available)
npx serve .
```

The speech recognition page (`home/speech-recognition.html`) requires Chrome or Edge — the Web Speech API is not supported in Firefox.

## Architecture

```
index.html                   # Root redirect → /home/ (JS + meta-refresh fallback)
home/
  index.html                 # Main portfolio (hero, about, skills, projects, automation, contact)
  speech-recognition.html    # Standalone speech-to-text tool (Web Speech API)
style.css                    # Shared dark-theme design tokens for sub-pages
CNAME                        # kueichi.dev (Cloudflare Pages custom domain)
```

**Two independent CSS design systems coexist:**
- `style.css` — shared tokens (`--accent: #e8a33d` amber, `--bg: #0f1115`) intended for sub-pages; uses system fonts
- `home/index.html` — fully self-contained inline CSS with its own token set (`--accent: #5fffb0` green, `--accent2: #5b8cff` blue; Space Mono + Syne fonts from Google Fonts). This page does **not** link to `style.css`.

New sub-pages under `home/` should import `style.css` and follow its token names. Do not change `home/index.html` to use `style.css` without a deliberate redesign decision.

## Content Placeholders Still Needing Updates

- `home/index.html` — n8n Automation section has 4 placeholder flow cards ("Add Your Flow Here")
- `home/index.html` — Contact email is `your@email.com` (placeholder)
- `home/index.html` — Resume link points to `/resume.pdf` (file not in repo)

## Scroll Animation Pattern

`.fade-in` elements animate on scroll via `IntersectionObserver` (threshold 0.1). Add the `fade-in` class to any section wrapper; visibility is toggled by adding `.visible` in JS at the bottom of `home/index.html`.

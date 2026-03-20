# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Rachel's Study Hub — a static, password-protected personal study site hosted on Vercel. No build step, no frameworks, no package manager. All files are plain HTML/CSS/JS.

## Deployment

```bash
vercel --prod        # deploy to production
```

Or push to the connected GitHub repo for automatic deploys.

## Architecture

### Page Flow
- `index.html` — Login page. Checks password client-side, writes `studyhub_auth = '1'` to `sessionStorage` on success, then redirects to `app.html`.
- `app.html` — Main hub. Auth-gated on load. Displays all guide cards. Reads/writes card statuses to `localStorage` under key `studyhub_statuses`.
- `guides/*.html` — Individual study guides. Each must include an auth guard at the bottom before `</body>`.

### Auth System
Client-side only. The password lives in `index.html` as `const CORRECT = '...'`. Sessions are `sessionStorage`-based (clears on browser close). Every protected page redirects to `index.html` if `studyhub_auth !== '1'`.

### Card Status Persistence
Card statuses (Not Started / In Progress / Complete) are saved in `localStorage` by `data-id` attribute. Status keys: `new`, `active`, `done`.

## Adding a New Study Guide

1. Place the HTML file in `guides/`
2. In `app.html`, copy an existing `<a class="guide-card">` block between the marked comments and update: `href`, `data-id` (unique), `h4`, `.desc`, `.card-meta` fields, color class (`c-blue` | `c-green` | `c-purple` | `c-amber`), icon emoji, and `status-pill` class
3. Add the auth guard to the new guide file before `</body>`:
   ```html
   <script>
     if (sessionStorage.getItem('studyhub_auth') !== '1') {
       window.location.replace('../index.html');
     }
   </script>
   ```
4. Deploy

## Design System

Fonts (loaded from Google Fonts): `DM Serif Display` (headings), `Outfit` (body), `JetBrains Mono` (code/labels).

CSS custom properties defined in `:root` on each page — `--bg`, `--surface`, `--border`, `--accent` (blue), `--green`, `--amber`, `--purple`, `--text`, `--muted`.

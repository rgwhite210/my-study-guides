# Rachel's Study Hub

Personal certification prep workspace. Password-protected, session-based auth.

## Project Structure

```
/
├── index.html                        ← Login page
├── app.html                          ← Main hub (links to all guides)
├── CIW_HTML5_CSS3_Study_Guide.html   ← Study guide (add more like this)
├── vercel.json                       ← Vercel config
└── README.md
```

## Deploy to Vercel

### Option A — Vercel CLI (quickest)
```bash
npm i -g vercel       # install once
vercel                # first deploy (follow prompts)
vercel --prod         # subsequent deploys
```

### Option B — GitHub (recommended for ongoing use)
1. Push this folder to a GitHub repo (can be private)
2. Go to vercel.com → New Project → Import that repo
3. Click Deploy — done. Every `git push` auto-deploys.

## Adding a New Study Guide

1. Drop the new `.html` file in the project root
2. Open `app.html` and find the comment:
   `<!-- ── GUIDE CARDS — add new ones below this line ── -->`
3. Copy an existing `<a class="guide-card">` block and update:
   - `href` → filename of new guide
   - `h4` → title
   - `.desc` → description  
   - `.card-meta` divs → exam ID, question count, domains
   - Color class: `c-blue` | `c-green` | `c-purple` | `c-amber`
   - Icon emoji
4. Change `status-pill` class: `pill-new` | `pill-active` | `pill-done`
5. Deploy

## Changing the Password

Open `index.html` and find:
```js
const CORRECT = 'password123';
```
Change the value and redeploy.

## How Auth Works

- Login checks password client-side → stores `studyhub_auth = '1'` in `sessionStorage`
- Every protected page checks for that key on load → redirects to login if missing
- Session clears automatically when browser closes (sessionStorage behavior)
- To add auth guard to a new study guide, paste this at the bottom before `</body>`:

```html
<script>
  if (sessionStorage.getItem('studyhub_auth') !== '1') {
    window.location.replace('index.html');
  }
</script>
```

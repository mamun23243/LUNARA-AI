# LUNARA AI — Gemini + GitHub Pages + Railway

This package is prepared for a split deployment:

- **GitHub Pages:** frontend in `docs/`
- **Railway:** Node.js/Gemini backend in the project root
- **Gemini API key:** server-side only in Railway environment variables

## 1) Run locally

Install Node.js LTS first.

```bash
npm install
```

Copy `.env.example` to `.env` and add your Gemini key:

```env
GEMINI_API_KEY=YOUR_REAL_KEY
GEMINI_MODEL=gemini-2.5-flash
PORT=3000
```

Then:

```bash
npm start
```

Open `http://localhost:3000`.

## 2) Deploy backend to Railway

1. Push this entire folder to a GitHub repository.
2. In Railway, create a new project from that GitHub repository.
3. Railway will detect Node.js and use `npm start`.
4. Add these Variables in Railway:

```text
GEMINI_API_KEY=YOUR_REAL_KEY
GEMINI_MODEL=gemini-2.5-flash
FRONTEND_ORIGINS=https://YOUR-USERNAME.github.io/YOUR-REPO
```

5. Deploy.
6. Open Railway's generated public domain and test:

```text
https://YOUR-RAILWAY-DOMAIN/api/health
```

It should show `ok: true` and `geminiConfigured: true`.

## 3) Deploy frontend to GitHub Pages

The GitHub Pages copy is in `docs/`.

Before enabling Pages, edit:

```text
docs/config.js
```

Change:

```js
window.LUNARA_API_URL = "https://YOUR-RAILWAY-APP.up.railway.app";
```

to your real Railway public URL, with no trailing slash.

Then push to GitHub.

In GitHub:

`Settings -> Pages -> Deploy from a branch -> main -> /docs`

Save. GitHub will provide your Pages URL.

## 4) Important

Never put `GEMINI_API_KEY` inside `docs/`, `public/`, `index.html`, or any GitHub file.

The GitHub Pages frontend calls Railway, and Railway calls Gemini.

The existing login/signup is browser/localStorage demo authentication. For production authentication, add a real database and server-side auth.

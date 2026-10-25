# Modern Portfolio — GitHub Pages + secure GitHub editor

Responsive portfolio with glassmorphism, dark/light theme, cursor spotlight, scroll reveals, profile, logo/branding, skills, experience, projects, repository-hosted videos, social links and a GitHub-connected editor.

## Architecture and security
GitHub Pages hosts the static website. A Cloudflare Worker handles GitHub OAuth and commits editor changes; Cloudflare KV stores temporary OAuth state and server-side sessions. GitHub access tokens are never returned to browser JavaScript; the browser receives only an opaque `HttpOnly; Secure; SameSite=Lax` cookie. The Worker allowlists usernames and writes only to the configured repository.

**Production security:** this starter uses OAuth scope `public_repo`, which can grant write access to other public repositories accessible to the authorizing user. For least privilege, replace this with a GitHub App restricted to the portfolio repository and Contents permissions. Never put OAuth secrets or access tokens in frontend code.

## 1. Publish to GitHub Pages
1. Create a public repository, e.g. `my-portfolio`, and upload this project to `main`.
2. Open repository **Settings → Pages**; select **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. A project site looks like `https://USERNAME.github.io/my-portfolio/`; a user site repository named `USERNAME.github.io` uses `https://USERNAME.github.io/`.
4. Replace example details in `data/portfolio.json`. Sample links and video paths are placeholders.

## 2. Create GitHub OAuth App
Go to GitHub **Settings → Developer settings → OAuth Apps → New OAuth App**. Set homepage to your Pages origin, e.g. `https://USERNAME.github.io`. Set callback URL to the Worker URL ending `/auth/callback`. Copy Client ID and create Client Secret; store both only as Cloudflare Worker secrets. Starter scope is `public_repo read:user`; do not use it for private repository access.

## 3. Deploy the Worker
Install current Node.js LTS, then run:
```bash
cd worker
npm install
npx wrangler login
npx wrangler kv namespace create SESSIONS
```
Paste the returned namespace ID into `worker/wrangler.toml` replacing `REPLACE_WITH_KV_NAMESPACE_ID`. Edit the variables there: `GITHUB_OWNER`, `GITHUB_REPO`, `GITHUB_BRANCH`, `ALLOWED_ORIGIN` (origin only, no path/trailing slash), `ALLOWED_GITHUB_USERS` (lowercase usernames), and `OAUTH_CALLBACK_URL`. For a project Pages URL, `ALLOWED_ORIGIN` is still `https://USERNAME.github.io`.

Set secrets without committing them:
```bash
npx wrangler secret put GITHUB_CLIENT_ID
npx wrangler secret put GITHUB_CLIENT_SECRET
npx wrangler deploy
```
Paste each value when prompted.

## 4. Connect frontend
In `js/app.js`, replace `const API=window.PORTFOLIO_API_BASE||"";` with `const API=window.PORTFOLIO_API_BASE||"https://YOUR-WORKER.workers.dev";`. This URL is public, not a secret. Commit the change and wait for Pages deployment.

## 5. Use the editor
Open the live site → **Edit portfolio** → **Connect with GitHub**. Edit profile and branding fields; skills, experience, projects, videos and social links are JSON arrays. Click **Publish changes**; the Worker commits `data/portfolio.json`, and Pages may take a few minutes to redeploy. Editing the JSON directly in GitHub is a useful fallback.

Logo uploads support PNG/JPEG/WebP/SVG under 700 KB and commit to `assets/logos/`. Only upload trusted SVG files. Video metadata can be edited in the dashboard, but video binaries must be committed/uploaded to `assets/videos/` through GitHub separately. Reference them as `assets/videos/demo.mp4`. GitHub blocks individual files over 100 MB and Pages has a published-site size limit; compress clips or use external hosting for large libraries.

## Local preview
```bash
python3 -m http.server 8000
```
Open `http://localhost:8000`. The public site works without a Worker; only editor publishing needs it.

## Troubleshooting
- Publishing service not configured: set `API` in `js/app.js`.
- CORS/origin: `ALLOWED_ORIGIN` must exactly match site origin.
- OAuth callback: GitHub callback URL and `OAUTH_CALLBACK_URL` must match exactly.
- Account not allowed: add the exact lowercase login to `ALLOWED_GITHUB_USERS`.
- No visible update: check the repository Actions tab for Pages deployment and hard-refresh.

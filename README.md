# Modern Portfolio — GitHub Pages

A responsive, static portfolio website with glassmorphism, dark/light theme, cursor spotlight, scroll animations, projects, video demos, profile, skills, experience, and contact links. No backend, Cloudflare Worker, Wrangler, OAuth, database, or API setup is required.

## Publish on GitHub Pages
1. Upload the contents of this folder to the root of your GitHub repository and commit them to `main`.
2. Open the repository **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/(root)`, then save.
4. Wait for the Pages deployment to finish.

## Update your portfolio content
All portfolio text and content are stored in `data/portfolio.json`. To update your website: open that file in your GitHub repository, click the pencil/Edit button, change the values, then click **Commit changes**. GitHub Pages will rebuild the site automatically. You can also edit the file locally and push it to GitHub.

Common fields include:
- `profile`: name, role, introduction, about text, email, résumé URL and footer text.
- `brandName`, `logoPath`, `siteTitle`, and `theme`: branding and accent colors.
- `social`: social profile links.
- `skills`: grouped skill lists.
- `experience`: work or project timeline.
- `projects`: project title, category, description, tags, GitHub URL, demo URL and optional image/video.
- `videos`: video title, description, video path and optional poster image.

Keep the JSON valid: use double quotes around strings and do not leave trailing commas. If you change `logoPath`, upload the logo into the repository too. Images can go in `assets/images/`. **Do not upload private/original video files to a public GitHub repository.** Public MP4 files and any media URL used by a browser can be copied by visitors.

## Preview locally
From this folder, run:
```bash
python3 -m http.server 8000
```
Then open `http://localhost:8000`.

## Notes
- This is a static website. Visitors cannot edit the site through the webpage; only repository contributors with GitHub access can change the JSON or assets.
- Dark/light theme selection is saved in each visitor's browser.


## Video protection setup (important)

The video section is prepared for **Cloudflare Stream signed playback** via a separate authorization endpoint. GitHub Pages is static and cannot safely create signed tokens itself.

1. Upload video files to Cloudflare Stream (or another streaming provider that supports signed playback). Do not upload originals to this public repo.
2. Configure the provider to require signed URLs/tokens for the videos.
3. Deploy a server-side authorization endpoint. It should validate allowed video IDs, authorize the request as needed, create a short-lived signed playback URL/token using a secret stored server-side, and return JSON like `{"playbackUrl":"https://...signed-playback-url..."}`. Never put provider API tokens or signing secrets in `data/portfolio.json` or browser JavaScript.
4. In `data/portfolio.json`, set `videoSecurity.playbackEndpoint` to your deployed endpoint URL and replace each `videoId` with the real provider video ID.
5. Test the endpoint's CORS policy for your GitHub Pages domain. Use HTTPS only.

The browser requests a short-lived playback URL when a viewer presses Play. The endpoint must implement the actual provider-specific signing; this repository deliberately does not include secrets or pretend to secure videos without that server-side step. If you do not configure an endpoint, secure-stream playback will show a configuration message.

### What this does and does not protect

- The player hides its normal download control where supported, and the UI adds a preview watermark.
- Signed playback can make direct-link sharing and casual downloading harder when tokens are short-lived and provider enforcement is enabled.
- It **cannot** prevent screen recording, camera recording, or every form of media capture. Browser-visible video must be delivered to the viewer to play.
- A watermark rendered by this website is a deterrent, not a forensic watermark; it may be cropped out. For stronger deterrence, burn a visible watermark into preview files before uploading them to the streaming provider.
- Do not use direct `assets/videos/*.mp4` URLs for content you need to protect. A static file hosted in a public repository is public.

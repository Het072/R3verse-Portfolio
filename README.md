# Modern Portfolio — GitHub Pages

A responsive, static portfolio website with glassmorphism, dark/light theme, cursor spotlight, scroll animations, video demos, profile, skills, experience, and contact links. The public site is static GitHub Pages. Admin Studio uses the GitHub Contents API to publish edits, so you must connect a repository-scoped GitHub token.

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


## Admin Studio and video uploads
1. Visit `/admin/`, sign in, and connect your repository using a fine-grained GitHub personal access token with **Contents: Read and write** access to that repository only.
2. Choose **Video gallery** in the side panel. Upload a compressed MP4 or WebM preview (recommended under 50 MB), enter its title/description, and click **Upload video & add to gallery**.
3. Click **Save & publish** to publish the gallery metadata. GitHub Pages will update after the commit deploys.
4. Use **Admin settings** to change the local admin password. This static-site password is stored in this browser only; clearing browser storage resets it to the default. It is a convenience lock, not server-side authentication. Do not rely on it to protect secrets.

## Important video note
GitHub Pages videos in a public repository are publicly accessible and can be downloaded. Upload only compressed preview copies, not private originals. The video slider automatically changes preview every 10 seconds. Selecting a tile opens the full player in a glass-effect modal. No Cloudflare video-security setup is used in this version.

## Preview locally
Run `python3 -m http.server 8000` from the project root, then open `http://localhost:8000`.

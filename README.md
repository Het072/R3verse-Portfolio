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

Keep the JSON valid: use double quotes around strings and do not leave trailing commas. If you change `logoPath`, upload the logo into the repository too. Images can go in `assets/images/`; video files can go in `assets/videos/`. Reference them with paths such as `assets/images/project-one.png` or `assets/videos/demo.mp4`. Compress videos before uploading; GitHub rejects individual files larger than 100 MB.

## Preview locally
From this folder, run:
```bash
python3 -m http.server 8000
```
Then open `http://localhost:8000`.

## Notes
- This is a static website. Visitors cannot edit the site through the webpage; only repository contributors with GitHub access can change the JSON or assets.
- Dark/light theme selection is saved in each visitor's browser.

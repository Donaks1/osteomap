# Deploying OsteoMap

This guide explains how to host your own copy of OsteoMap. You don't need anything from the original developers, the Centre for Forensic Anthropology or The University of Western Australia to do this.

OsteoMap is a static website. Building it produces a folder of ordinary files (HTML, JavaScript, CSS and images), and any web host that serves files over HTTPS can host it. There is no server program, database, user account, API key or other secret to set up.

Choose one way to deploy:

- [Option A: GitHub Pages](#option-a-github-pages). Free, and GitHub builds and publishes the site for you. You only need a GitHub account and a web browser.
- [Option B: another static web host](#option-b-another-static-web-host), such as Netlify, Cloudflare Pages or Vercel, which build the site from your GitHub repository.
- [Option C: build it yourself and upload the files](#option-c-build-it-yourself-and-upload-the-files) to any web server, such as one run by your organisation.

Before your copy goes live, [replace the logo icons](#before-you-deploy-replace-the-logo-icons). After it does, [check that it works](#check-your-deployment).

> **Records stay with the web address.** OsteoMap stores records in each user's browser, for the web address it was opened from. If your users already have records in another copy of OsteoMap, or if you later move your copy to a different address, they need to export a full backup at the old address and import it at the new one. See [DISCLAIMER.md](../DISCLAIMER.md#data-storage-and-backups).

## Before you deploy: replace the logo icons

The app icons show the Centre for Forensic Anthropology logo, which is not part of OsteoMap's public-domain dedication (see [Licence](../README.md#licence)). Replace these files in `osteomap/public/` with your own artwork, keeping the same names and pixel sizes:

| File | Size (pixels) | Used for |
|------|---------------|----------|
| `favicon.png` | 64 × 64 | Browser tab |
| `apple-touch-icon.png` | 180 × 180 | iPhone and iPad Home Screen |
| `pwa-192x192.png` | 192 × 192 | Installed app |
| `pwa-512x512.png` | 512 × 512 | Installed app and splash screen |
| `pwa-maskable-512x512.png` | 512 × 512 | Android, which crops it to a circle or rounded square, so keep the artwork within the middle 80% |

On GitHub, you can replace a file by opening the `osteomap/public` folder, choosing **Add file › Upload files**, and uploading files with the same names.

To change the app's name, edit `name` and `short_name` in `osteomap/vite.config.ts`, and the `<title>` and `apple-mobile-web-app-title` in `osteomap/index.html`.

## Option A: GitHub Pages

The repository includes a workflow (`.github/workflows/deploy.yml`) that tests and builds OsteoMap and publishes it to GitHub Pages every time the `main` branch changes. It works out the site's web address by itself, so it needs no changes for your account.

1. **Make your own copy of the repository.** Sign in to GitHub and do one of the following:
   - Open the [OsteoMap repository](https://github.com/Donaks1/osteomap) and choose **Fork**, then **Create fork**.
   - Or, to make a copy that isn't linked to the original, choose **Code › Download ZIP** and unzip it. Then [create a new repository](https://github.com/new) and upload the unzipped files to it, keeping the folder structure. Afterwards, check that `.github/workflows/deploy.yml` is in your repository: many computers hide folders whose names start with a dot, so it is easy to leave out. If it is missing, create the file on GitHub and paste in its contents.
2. **Replace the logo icons** in your copy, as described [above](#before-you-deploy-replace-the-logo-icons).
3. **Turn on GitHub Pages.** In your repository, open **Settings › Pages**. Under **Build and deployment**, set **Source** to **GitHub Actions**.
4. **Allow the workflow to run.** Open the **Actions** tab. If GitHub says workflows are disabled (it does for forks), choose **I understand my workflows, go ahead and enable them**.
5. **Publish.** In the **Actions** tab, choose **deploy** in the list on the left, then **Run workflow** › **Run workflow**. From now on, every change to `main` publishes again automatically.
6. **Open your site.** When the run finishes (a few minutes), its address is shown on the run's summary page and in **Settings › Pages**. It is usually `https://<your-username>.github.io/<repository-name>/`.

Notes:

- GitHub Pages sites are public on the internet, even when the repository is private (and Pages for private repositories needs a paid GitHub plan). Records are still stored only on each user's device.
- To use your own domain, set it under **Settings › Pages › Custom domain**, then run the **deploy** workflow again so the app is rebuilt for the new address. Users will need to move their records across, as explained at the top of this guide.
- If the run fails at **configure-pages**, GitHub Pages hasn't been turned on (step 3).

## Option B: another static web host

Hosts such as [Netlify](https://www.netlify.com/), [Cloudflare Pages](https://pages.cloudflare.com/) and [Vercel](https://vercel.com/) can build OsteoMap straight from your copy of the repository on GitHub. Make your copy and replace the icons as in steps 1 and 2 of Option A, connect the repository to the host, and use these build settings:

| Setting | Value |
|---------|-------|
| Base (root) directory | `osteomap` |
| Build command | `npm run build` (these hosts install the packages first) |
| Publish (output) directory | `dist` (that is, `osteomap/dist`) |
| Node.js version | 24 (also recorded in `osteomap/.nvmrc`) |

If your host doesn't use GitHub Pages' workflow, you can delete `.github/workflows/deploy.yml`.

## Option C: build it yourself and upload the files

You need [Node.js](https://nodejs.org/) 24 or later, and a copy of the repository on your computer (from **Code › Download ZIP**, or `git clone`).

```bash
cd osteomap
npm ci                # installs the exact package versions in package-lock.json
npm test              # optional: runs the automated tests
npm run build         # builds the site into osteomap/dist
npm run preview       # optional: serves the built site at http://localhost:4173 to try it
```

Upload everything inside `osteomap/dist` (not the folder itself) to your web server.

By default the build expects to be served from the root of a domain (`https://example.org/`). To serve it from a sub-folder, build it for that path, including the slashes at both ends:

```bash
npm run build -- --base=/osteomap/
```

Use `npm run build` and `osteomap/dist` for real use. `npm run dev` starts a development server, which is only for working on the code.

## Web server requirements

These apply to any host. Options A and B already meet them.

- **Serve over HTTPS.** Browsers only allow the offline mode and the **Install** option on secure sites. (`http://localhost` is the one exception, for testing.)
- **Serve the files as they are.** OsteoMap is a single page with no server-side routes, so no redirect or rewrite rules are needed.
- **Don't cache the update files for long.** `index.html`, `sw.js`, `registerSW.js` and `manifest.webmanifest` should be revalidated on every visit (for example, `Cache-Control: no-cache`), or users may not receive updates. Files in `assets/` have a content hash in their names and can be cached indefinitely.

## Check your deployment

1. Open the site's address. Check the icon in the browser tab, and open **Help** to check the **About OsteoMap** section.
2. Add a test site, context and specimen, mark a few bones, then reload the page and check that everything is still there.
3. Open **Import / Export**, choose **Export Full Backup (.csv)**, and check that a file downloads.
4. On a phone, follow **Help** to install the app, then open it with the network turned off to check that it works offline.
5. Delete your test records.

## Updating your copy

To take later changes from the original repository into a fork, use **Sync fork** on your repository's main page. (A copy that isn't a fork has to be updated by hand.) Changes to `main` are published automatically with Option A, and by most hosts with Option B. With Option C, build and upload again.

Users' records are kept through updates, and an installed app updates itself the next time it is opened with an internet connection. Even so, ask users to export a full backup before a significant update.

## Licence

OsteoMap is in the public domain ([CC0 1.0](../LICENSE)), so you don't need permission to deploy, change or share it, and you don't have to credit anyone (although an [acknowledgement](../README.md#acknowledging-osteomap) is appreciated). Only two things carry conditions:

- **Third-party packages.** The build writes the licences of the open-source packages it includes to `third-party-licenses.txt`, which is served with the app. Keep it there.
- **Names and logos.** Replace the logo icons, and don't use the names or logos of The University of Western Australia or the Centre for Forensic Anthropology to suggest they endorse your copy.

The [disclaimer](../DISCLAIMER.md) applies to your copy too, and you are responsible for running it. If you collect information about your users, or your hosting provider does (most record visitors' IP addresses), you may need a privacy notice of your own.

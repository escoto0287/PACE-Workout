# PACE — Android installable workout app (PWA)

This package wraps the existing **PACE 4.0 workout player and compact editor** into an installable PWA (4.1), preserving all 11 workout templates and five workout methods.

## Deploy (one time)

**Azure Static Web Apps** (good fit if you already use Azure):

1. Create a GitHub repository or choose one intended for PACE.
2. Put the **contents** of this ZIP (not the ZIP itself) at the repository root, including `index.html`, `manifest.webmanifest`, `sw.js`, and `icons/`.
3. Create an Azure Static Web App, linked to that GitHub repository and its `main` branch.
4. Set the source/app location to `/` and the output location to blank. This is a static HTML site; there is no build step.
5. Azure gives you an HTTPS address. Visit it in Chrome on Android.
6. Open Chrome's **⋮** menu → **Install app** (or **Add to Home screen**, depending on the browser and installation criteria). Confirm. Then launch PACE from the phone's launcher.

**Alternative**: Cloudflare Pages, GitHub Pages, or any HTTPS static host. Host at root or a consistent subpath. Do not open `index.html` as a downloaded file: an installable service worker requires HTTPS (localhost is for development).

## Updates (no new phone HTML downloads)

Make changes in the repository, then deploy the updated files to the **same HTTPS address**. Increment `CACHE_NAME` in `sw.js` with each release. The installed app detects a waiting service worker and offers **Update now** when it discovers a new version. On future launches it checks for updates. User plans remain in origin-specific browser storage.

**Your workout player hasn't been redesigned**: the same five methods, built-in workouts, block editor, audio/vibration options and two-week schedule remain.

## Workouts and data safety

- Data is saved locally **per browser/origin**, not synced to ChatGPT, Azure, or other devices.
- If you previously used the standalone `pace_interval_studio_v4.html`, its storage will **not automatically move** to this installed PWA. In that old file use **Export plan**, then in the installed app use **Import plan** for the active workout.
- The original v4 file exports only the active plan. Saved custom routines and completed calendar checkmarks do **not** move automatically; recreate those or separately export any individual routine by loading then exporting it.
- This 4.1 PWA adds **Backup all data** / **Restore backup** for the active workout, saved library, and two-week checkmarks. Make a backup before changing phones, reinstalling, or clearing browser data.
- Installed PWAs may be eligible for automatic storage cleanup by the OS/browser. They are not cloud sync.
- For accurate beeps/vibration and screen wake-lock, keep the app visible and the screen on while training. Android background execution and lock-screen timing may be constrained.

## AI editing

This version still has ChatGPT copy/paste workout JSON. **Installing a PWA does not create an AI backend.** For natural-language edits to work entirely inside PACE, a future release needs an authenticated server-side API to a model; never put private API keys in browser JavaScript.

## Local test

Run `python -m http.server 8080` in the unzipped folder, then open `http://localhost:8080` **on the same computer**; `localhost` refers to the current device. Testing installation on a phone should be done on the public HTTPS deployment.

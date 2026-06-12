# Momentum PWA — Deploy Guide

Your Momentum app, packaged as an installable Progressive Web App. Seven files, no build step needed on your end, no server, no dependencies — everything is bundled into `app.js`.

## What's in this folder

| File | Purpose |
|---|---|
| `index.html` | App shell — loads the bundle, registers the service worker |
| `app.js` | The entire app (React + icons bundled, 227 KB, minified) |
| `manifest.webmanifest` | Makes Android treat it as an installable app |
| `sw.js` | Service worker — caches the app so it works offline |
| `icon-192.png`, `icon-512.png`, `icon-512-maskable.png` | App icons |

## Deploy to GitHub Pages (≈10 minutes)

1. Go to https://github.com/new and create a repository, e.g. `momentum`. Public is required for free Pages hosting.
2. On the new repo page, click **uploading an existing file**, drag all 7 files from this folder in, and commit.
3. Go to **Settings → Pages**. Under "Build and deployment", set Source to **Deploy from a branch**, pick `main` and `/ (root)`, and Save.
4. Wait 1–2 minutes. Your app is live at `https://<your-username>.github.io/momentum/`

## Install on your Android phone

1. Open the URL in **Chrome** on your phone.
2. Tap the **⋮ menu → Add to Home screen** (or "Install app" if Chrome offers it).
3. Confirm. Momentum now opens full-screen from its own lavender icon, works offline, and keeps its own data.

## Where your data lives

Everything is stored in your browser's localStorage **on the device** — nothing is sent anywhere. Two consequences:

- Phone and laptop each have their own independent data.
- Clearing Chrome's site data deletes it. **Use Settings → Backup & restore → Export backup** regularly. The exported JSON can be imported on any device to move or restore your data.

## Migrating your data from the Claude artifact (optional)

If you've logged meaningful history in the artifact version, open that artifact in claude.ai and ask Claude (in that chat) to add a temporary export button, or paste this into the artifact chat:

> "Add a temporary button to Settings that downloads all of momentum:config, momentum:tasks, and momentum:sessions from window.storage as a single JSON file shaped like `{app:'momentum', config:{buckets,weeklyTarget}, tasks:[...], sessions:[...]}`"

The downloaded file imports directly via **Settings → Import backup** in this PWA.

## Updating the app later

When you want changes, edit and re-upload `app.js` (Claude can rebuild it for you), then bump `CACHE_VERSION` in `sw.js` (e.g. `momentum-v2`) so installed phones pick up the new version on next launch.

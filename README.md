# Command Center

A single-file personal command center bundling three tools behind one dark Material-Design shell:

- **Garage Fund** — savings goal tracker with a ledger, debts (owed / I owe), monthly budgets, a reselling "flips" pipeline, Shein-affiliate commissions in ZAR, and a linear-regression forecast.
- **Study Command Center** — lessons, todos, streaks.
- **Calculator** — basic arithmetic with history and keyboard support.

## Where data lives

The code is a plain static HTML file — the deploy just serves it. All your data is kept in the visiting browser's `localStorage`, which means:

- Every browser and every device starts empty. Chrome on your laptop ≠ Safari on your phone.
- Private/incognito windows start empty and forget on close.
- Clearing site data wipes it.

To move between devices or recover from a wipe, use **Settings → Export JSON** on the source and **Settings → Import JSON** on the destination. The dashboard shows a nag banner if it's been more than 30 days since the last Export.

## Run locally

Open [`index.html`](./index.html) in a browser. No build step, no dependencies beyond Google Fonts (loaded from CDN).

## Deploy

This repo is Vercel-ready. Push to GitHub, then in the Vercel dashboard click **Add New → Project**, pick the repo, leave every setting at default (Framework Preset = Other) and deploy. [`vercel.json`](./vercel.json) sets clean URLs, no-cache for `index.html` (so redeploys are seen instantly), and sensible security headers.

## Install as an app (PWA)

Once deployed over HTTPS, the site is a Progressive Web App:

- **Desktop (Chrome, Edge, Brave, Arc)** — an install icon appears in the URL bar. Click it to install.
- **Android (Chrome, Samsung Internet)** — you'll see an "Install app" prompt or an "Add to home screen" option in the menu.
- **iOS Safari** — tap the Share button, then "Add to Home Screen".

Once installed, the app opens standalone (no browser chrome), works offline (cached by a service worker), and shows up in your device's app switcher like any other app. Your data still lives in the same per-browser `localStorage` — installing doesn't move or sync it.

## Data safety

- Every Import first snapshots the current state to `garageFund.preImport.<timestamp>` (keeps the last 3), so a wrong-file mistake is recoverable via DevTools.
- The dashboard shows a nag banner after 30 days without an Export.
- A welcome banner points first-time visitors on a fresh browser to Import if they have a JSON from elsewhere.
- `*.json` is gitignored so raw ledger exports never end up in the repo.

## Schema

`localStorage['garageFund.v5.1']` holds the Garage Fund state. The key name is frozen at `v5.1` even though the current schema is `5.5` — the `Storage.migrate()` ladder upgrades transparently, and freezing the key means older builds can still read the same slot. Study Command Center uses its own separate key (`studyCommandCenterV1`).

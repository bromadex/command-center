# Command Center

A single-file personal command center bundling three tools behind one dark Material-Design shell:

- **Garage Fund** — savings goal tracker with a ledger, debts (owed / I owe), monthly budgets, a reselling "flips" pipeline, Shein-affiliate commissions in ZAR, and a linear-regression forecast.
- **Study Command Center** — lessons, todos, streaks.
- **Calculator** — basic arithmetic with history and keyboard support.

All state lives in the visiting browser's `localStorage` — nothing is sent to any server. Use **Settings → Export JSON** regularly.

## Run locally

Just open [`index.html`](./index.html) in a browser. No build step, no dependencies beyond Google Fonts (loaded from CDN).

## Deploy

This repo is Vercel-ready. Push to GitHub, then in the Vercel dashboard click **Add New → Project**, pick the repo, and deploy — the [`vercel.json`](./vercel.json) sets clean URLs and sensible cache headers for the static HTML.

## Data safety

- The app auto-snapshots your current state to a `garageFund.preImport.*` localStorage key before every Import, keeping the last 3.
- The dashboard shows a nag banner if you haven't exported in 30 days.
- `*.json` is gitignored so raw ledger exports never end up in the repo by accident.

## Schema migrations

`localStorage['garageFund.v5.1']` holds the Garage Fund state. The key name is frozen at `v5.1` even though the current schema is `5.5` — the `Storage.migrate()` ladder handles upgrades transparently, and freezing the key means older builds can still read the same slot.

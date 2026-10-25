# Personal Command Center

A single-file personal command center in a purple liquid-glass (iOS 26 inspired) interface. Manage money, debts, flips and financial targets, capture ideas, plan study and keep book notes.

| Section | What it holds |
| --- | --- |
| **Overview** | Balance, income/expenses, debts, target progress, charts, recent activity, priorities, quick actions |
| **Money** | Transactions (income, expense, transfers, recurring), budgets, forecast, Shein affiliate earnings |
| **Debts** | Owed to you / you owe, partial settlement, overdue tracking |
| **Flips** | Inventory, purchases, sales, realized vs projected profit |
| **Targets** | Flexible goals; allocate money as a planning earmark |
| **Ideas** | Idea board with status, priority, category, tags; turn an idea into a target |
| **Study** | Weekly plan plus the Study Center (PMP, PTE, CODESYS, Bible, Reading log, To-Do) |
| **Books** | Books with status/progress and important notes (title, insight, details, chapter/page, tags) |
| **Calculator** | Arithmetic with history and keyboard support |
| **Settings** | Starting balance, full export/import, study-only backup, danger zone |

## Where data lives

Plain static HTML. All data stays in the browser's `localStorage`; it does **not** sync between browsers or devices.

- `garageFund.v5.1` — finance, targets, ideas, study plan, books (key name frozen so existing data and old backups keep working)
- `studyCommandCenterV1` — Study Center

Use **Settings → Export JSON / Import JSON** to move data or recover from a wipe. The export is one file containing both stores. The Overview nags if there has been no export for 30 days.

## Money model (unchanged)

- Available balance = starting balance + income + transfers in − expenses − transfers out.
- Debts, flip stock and target allocations are **never** counted as cash.
- Allocating money to a target is a planning earmark only. It creates no ledger row and cannot exceed the unallocated balance. Unallocated = balance − allocations.

## Data safety

- Schema **5.6** adds an explicit migration. The retired car goal stays in `goal` untouched and is copied to `retiredGoals`. Nothing is deleted.
- Before any migration, import or clear, a raw snapshot is stored (`garageFund.preMigrate.*`, `.preImport.*`, `.preClear.*`, plus `studyCenter.*` equivalents; newest few kept).
- Imports are validated and refused with a specific message when the file is malformed.
- `*.json` is git-ignored so exports never reach the repo.

## Run / deploy

Open `index.html`, or push to GitHub and import into Vercel (Framework Preset: Other). `vercel.json` sets no-cache for `index.html` and `sw.js`. The service worker is network-first for HTML and bumps its cache name (`command-center-v3-glass`) so old caches are removed. Google Fonts load from a CDN; icons stay hidden if the icon font cannot load.

## Install as an app

Desktop Chrome/Edge: install icon in the address bar. Android: "Install app". iOS Safari: Share → Add to Home Screen.

## Code map (all in `index.html`)

1. `<style id="cc-design">` — design tokens (`--cc-*`), shell, shared components. The Targets/Ideas/Books module renders in a shadow root and inherits these tokens.
2. Money engine script — `Utils`, `Storage` (migrations), `Store`, `Accounting`, `Render`, `Events`.
3. Study Center script.
4. Planner script (Targets, Ideas, Study plan, Books) — exposes `window.Planner`.
5. Calculator script.
6. Shell script — navigation, hash routing (`#/money/budgets`), global search (Ctrl/Cmd+K), quick add, Overview and its SVG charts.

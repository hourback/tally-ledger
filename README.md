# Tally Ledger

An offline-first phone app for tracking daily behaviors, mostly yes/no, tagged positive (build) or negative (avoid).

- **Fully offline.** It's a static PWA. A service worker caches the app, and all data stays in the browser's storage on your device.
- **Your raw data.** Export to CSV (one row per day per behavior) or a full JSON backup. Import either format, or any CSV with `date` and `behavior` columns.
- **Reporting.** Daily balance chart, per-behavior rates with change from the previous period, streaks, a 26-week calendar and a weekday breakdown.

## Install on your phone

1. Open the GitHub Pages URL in Safari (iOS) or Chrome (Android).
2. Choose Share → Add to Home Screen (iOS), or Install app (Android).
3. Open it once while online so it caches. After that it works with no network.

Export a JSON backup from the Data tab now and then, or turn on cloud backup there. Browser storage is tied to the device, and clearing site data erases it.

## Cloud backup

Optional. Sign in on the Data tab to keep snapshots in Supabase (project `tally-ledger`). The phone stays the main copy and the app works offline as before. A snapshot is sent when you tap Back up now, and automatically when the app opens online after changes (at most once an hour). Restore latest merges the newest snapshot into the device.

The Supabase URL and publishable key in `index.html` are public by design. Row-level security on the `backups` table lets each signed-in user read and write only their own rows, and the newest 60 backups per user are kept. Never put the secret (service_role) key in this repo.

## Data format

CSV columns: `date,behavior,polarity,value,logged_at,behavior_id`

JSON backup: `{ app, version, exported_at, behaviors: [{id,name,polarity,created,archived}], entries: [{id,b,d,t}] }`, where `b` is the behavior id, `d` is the local date (YYYY-MM-DD) and `t` is the time it was logged.

## Development

No build step. Serve the folder over HTTP (`python3 -m http.server`) and open it. When you change cached files, bump `CACHE` in `sw.js` so installed copies update.

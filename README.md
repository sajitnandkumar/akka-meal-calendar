# Meal Plan — static PWA

A today-first meal viewer for the kitchen. Opens to today's three meals, has a full-week tab and a grocery list, and a language toggle (English / Hindi / Kannada / Telugu). Installable to a phone home screen.

## How it works

- `index.html` — the whole app (HTML + CSS + JS). No build step.
- `week.json` — the week's meals + grocery list. **This is the only file you regenerate each week.**
- `manifest.json`, `sw.js`, `icon-*.png` — PWA plumbing (install + offline).

The app reads `week.json` on load. If it can't (e.g. opened as a local file), it uses a copy baked into `index.html` so it still shows something.

## Deploy on GitHub Pages

1. Create a repo, drop all these files in the root.
2. Settings → Pages → Source = `main` / root.
3. Open the URL on the phone → browser menu → **Add to Home screen**.

Share that URL with whoever cooks. They open it each morning; it shows today automatically (based on the phone's date).

## Regenerating the week

Each meal supports four languages:

```json
"breakfast": { "en": "...", "hi": "...", "kn": "...", "te": "..." }
```

Missing languages fall back to `en`. The sample has Monday in all four; the rest are English only — your weekly generator should fill all four.

**Option A — Cowork scheduled task (Sunday):** have it output JSON in exactly this schema and commit `week.json` to the repo (GitHub connector or a git command).

**Option B — GitHub Action + Claude API (fully automated):** a scheduled workflow calls the API with your rules, writes `week.json`, commits it. No desktop needed.

Keep the schema identical or the app won't read it.

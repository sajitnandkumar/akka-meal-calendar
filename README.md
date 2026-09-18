# Akka Meal Calendar 🍲

A self-updating weekly meal planner for the kitchen. Every Sunday morning, an
automated planner writes a fresh seven-day menu for a metabolic-health diet —
then this little app shows whoever is cooking exactly what's on the stove today,
in their language.

Open it on a phone and it lands on **today**: breakfast, lunch, dinner, plus the
fixed daily items (black coffee, a mid-morning fruit-and-nuts break, an evening
whey shake). Swipe over to the **week** view to see all seven days, or the
**grocery** tab for the week's shopping list. One tap switches everything
between **English, हिन्दी, ಕನ್ನಡ, and తెలుగు**.

## How the week plans itself

A scheduled Claude Code task runs every **Sunday at 7:00 AM** and follows the
playbook in [`MEAL_PLAN.md`](MEAL_PLAN.md):

1. **Pull** the repo so it starts from the latest week.
2. **Read** the outgoing `week.json`, so no dish from last week sneaks straight back in.
3. **Generate** a new week against the diet rules — ~2,300 kcal and ~210 g
   protein a day, uric-acid-safe proteins, low-GI grains only, vegetables at the
   centre of lunch and dinner, everything cooked on a Bangalore stovetop
   (no oven, no grill, nothing deep-fried).
4. **Validate** the file with a script: schema, all four languages present,
   protein caps, meal-spacing rules, and a banned-ingredient sweep. A plan that
   fails a check gets fixed, not committed.
5. **Commit and push** `week.json` — and the app everyone's phone points at is
   already up to date.

If the laptop was asleep on Sunday, the next run catches up and still plans the
*current* week rather than skipping ahead.

## What's in the repo

| File | Role |
|---|---|
| `index.html` | The entire app — HTML, CSS, and JS in one file, no build step |
| `week.json` | The current week's meals and grocery list (the only file that changes weekly) |
| `MEAL_PLAN.md` | The full spec the Sunday automation follows — diet rules, schema, validation |
| `manifest.json`, `sw.js`, `icon-*.png` | PWA plumbing: install to home screen, work offline |

The app fetches `week.json` on load. Opened as a plain local file with no
network, it falls back to a copy baked into `index.html` so it never shows a
blank screen.

## Putting it on a phone

The repo is served with GitHub Pages (`main` branch, root). Open the Pages URL
on the phone, then **Add to Home screen** from the browser menu — it installs
like an app, works offline, and always shows the right day based on the phone's
date. Share the URL with whoever cooks; there's nothing to update manually.

## Changing the diet

Don't hand-edit `week.json` — it gets replaced every Sunday. Instead, change
the rules in `MEAL_PLAN.md` (allowed fish, banned ingredients, protein targets,
fruit list…) and the next run will honour them. The JSON schema itself must stay
exactly as documented there, or the app won't read it.

You maintain a weekly meal plan for a metabolic-health diet in the
akka-meal-calendar repository, which is this task's working folder. Run the whole
job end to end without asking questions.

Before anything else, run `git pull` so you are working from the latest week.json.

STEP 1 - Read the current week.json at the repo root. Note every meal used in it so
the new plan respects the no-repeat-within-2-days rule below, and so the new week
differs meaningfully from it.

STEP 2 - Work out the date range. Check today's date with the `date` command, do not
assume it. Then:
- if today is Sunday, the plan is for tomorrow's Monday through the following Sunday
- on any other day, the plan is for THIS week: the most recent Monday through the
  coming Sunday
This matters because a missed run catches up late. A run that fires on Tuesday
because the laptop was shut must still plan the current week, not skip to the next.
Use the Monday for meta.generatedFor ("Week of <DD Mon YYYY>") and for the commit
message. If week.json already carries that exact generatedFor value, the week has
already been planned: stop and say so rather than regenerating it.

TWO PEOPLE, ONE MENU: the plan feeds two people. Every meal is the SAME DISH for
both (one cooking job), with different portions:
- "s" (Sajit): the full portions all the rules below describe (~2,300 kcal,
  ~210g protein/day).
- "v" (Valentina): ~1,500 kcal, ~65-75g protein/day — roughly HALF the protein
  anchor, egg and dairy portions, the same vegetables and salads, the same or a
  slightly smaller grain. Whey 0.5 scoop in the evening shake; nuts 5 almonds
  or 1-2 walnut halves. All the low-GI / low-sodium / no-sugar rules apply to
  her portions too.
Every meal object and fixed item except coffee is {"s": {4 langs}, "v": {4 langs}};
coffee stays a single 4-language object (identical for both).

STEP 3 - Generate a NEW week.json. Output valid JSON in EXACTLY this schema (same
keys, same nesting). Every language object has all four languages: en, hi (Hindi),
kn (Kannada), te (Telugu).

{
  "meta": { "generatedFor": "Week of <DD Mon YYYY>", "generatedAt": "<ISO 8601 timestamp with offset, from `date -Iseconds`>", "targets": "S ~2,300 kcal · 210g protein — V ~1,500 kcal · 70g protein" },
  "fixed": {
    "coffee":     { "en": "...", "hi": "...", "kn": "...", "te": "..." },
    "midmorning": { "s": {4 langs}, "v": {4 langs} },
    "evening":    { "s": {4 langs}, "v": {4 langs} }
  },
  "days": {
    "mon": { "breakfast": {"s": {4 langs}, "v": {4 langs}}, "midmorning": {"s":..., "v":...}, "lunch": {"s":..., "v":...}, "dinner": {"s":..., "v":...} },
    "tue": {...}, "wed": {...}, "thu": {...}, "fri": {...}, "sat": {...}, "sun": {...}
  },
  "grocery": {
    "Proteins": ["item (qty)", ...], "Veg & Greens": [...],
    "Grains": [...], "Fruit": [...], "Pantry": [...]
  }
}

Keep the fixed items the same each week (except the named fruits in midmorning,
which change weekly):
- coffee: Black coffee, no sugar
- midmorning: NAME the week's specific fruits in the text (e.g. "Guava, orange or
  kiwi") + chia water + 8 almonds OR 2-3 walnut halves (favour walnuts ~3x/week).
  Never write "1 low-GI fruit" or "rotate" — always name the actual fruits.
  ADDITIONALLY, every day carries its own "midmorning" (see schema) naming exactly
  ONE fruit + chia water + that day's nut. Walnuts go on exactly 3 non-adjacent
  days; almonds on the rest. The per-day fruit assignments spread the week's
  3-4 fruits so no fruit runs 3+ days straight.
- evening: 250ml low-fat (double-toned or skim) milk + 1.5 scoop whey shake for
  "s"; 200ml + 0.5 scoop for "v" (no sugar). Never full-cream milk — saturated
  fat. The overnight-oats breakfast uses the same low-fat milk.

CONTENT RULES (every meal):
- Daily total ~2,300 kcal, ~210g protein.
- No added sugar, honey, jaggery, or juice; no maida; nothing deep-fried.
- NO pancakes, NO tikkas, NO grilled dishes, NO bakes/baked dishes: there is no
  grill or oven, and pancakes are not possible. Stovetop and steaming only
  (tawa, kadai, pressure cooker, steamer).
- GRAINS: only small portions of millet / brown rice / quinoa / Kerala matta rice /
  whole-grain sourdough (1-2 slices) / rolled or steel-cut oats (~40g dry,
  breakfast only — NEVER instant or flavoured oats). NO white rice, NO maida,
  NO brown bread.
- URIC-ACID SAFE: NO organ meat, prawns, shellfish, sardines, mackerel, surmai,
  anchovies, tuna, true (Atlantic) salmon, barracuda/sheela.
  Across the week: MAX 3 chicken meals + MAX 2 fish meals — and both fish meals
  must use the SAME fish (one variety, one purchase for the week), spaced at
  least 2 days apart.
  Fish must be seabass, red snapper, rawas (Indian salmon), or white pomfret
  ONLY, steamed, ~180-200g. No basa (too little protein).
  Dairy and plant protein freely.
- Low sodium. No pickle, no papad, no packaged/processed food.
- VEGETABLES: build lunch and dinner around these - palak, methi, cabbage,
  bottle/ridge/snake/ash gourd, bitter gourd (karela), cauliflower, broccoli,
  amaranth, drumstick/moringa, bhindi, beans, cluster beans, capsicum, cucumber,
  brinjal, zucchini, tindora. Limit starchy veg (potato, yam, corn, peas, arbi).
- WEEKLY VEG SET: pick exactly 5 vegetables from the list above (1-2 of them
  leafy greens) and build ALL of the week's lunches and dinners from only those
  5, plus the always-available staples (onion, tomato, cucumber, ginger, garlic,
  green chilli, coriander/curry leaves, lemon). The 5 repeat freely across the
  week; only whole meals follow the 2-day rule. Rotate the set from week to
  week (work karela in regularly), so variety comes across weeks, not from a
  huge single-week grocery list.
- Normal Bangalore-kitchen ingredients; cook time up to ~1 hour is fine.

PER SLOT:
- breakfast ~40g protein, ALWAYS includes 150g hung curd or Greek yogurt
  (never plain dahi — it has about a third of the protein). High-protein
  overnight oats (~40g rolled oats soaked in milk + 150g Greek yogurt +
  0.5-1 scoop whey + chia, no sugar) is a good zero-cook option 1-2x/week.
- lunch: built around a PROTEIN ANCHOR (below), small or no grain.
- dinner: built around a PROTEIN ANCHOR, small grain, greens.
- PROTEIN ANCHOR = 150-200g of paneer / tofu / soya chunks / chicken / allowed
  fish, or 3-4 eggs, or ~200g cooked legumes (kala chana, rajma, lobia, whole
  moong) as the main dish itself. A thin dal, a small khichdi, or dal cooked
  into a grain dish is a side, NOT an anchor. Every lunch and dinner must name
  its anchor with a gram (or egg-count) quantity.
- GRAVY NEEDS A CARRIER: any curry / gravy / stew / masala main must include a
  grain carrier in the same meal (1 millet roti or ⅓ bowl of an allowed rice or
  quinoa) — you cannot eat a gravy alone. Only dry or semi-dry mains (sukka,
  pepper-fry, stir-fry, bhurji, kadai, keema, sundal, bowls, salads) may be
  marked "(no grain)".

VARIETY:
- A meal MAY repeat within the week, but NOT within 2 days of its previous use
  (never on consecutive days, and not with only one day between).
- Must differ meaningfully from the previous week's plan (Step 1).
- Rotate the main protein across the week: eggs, paneer, tofu, curd, whey, chana,
  moong, soya, sattu, chicken; fish max 2 (same fish both times).
- WITHIN A DAY, no protein twice: breakfast, lunch and dinner must each use a
  DIFFERENT main protein (an egg breakfast rules out an egg dinner; a paneer
  component at breakfast rules out a paneer lunch). Only the constant dairy
  base is exempt (hung curd / Greek yogurt / milk / whey shake).
- Vary formats: chillas, bowls, parathas, stir-fries, curries, steamed dishes,
  slow-cooked dishes.
- FRUIT: choose 3-4 for the week from guava, apple, pear, plum, peach, apricot,
  jamun, orange, mosambi, kiwi, papaya, pomegranate, berries — and name them
  explicitly in the fixed midmorning text. Vary the choices from week to week.
  NEVER use: mango, ripe banana, chikoo, lychee, jackfruit, dates, dried fruit.

TRANSLATION: transliterate dish names (e.g. paneer bhurji -> పనీర్ భుర్జీ), translate
the connecting words and portions ("no grain", "with", "sautéed"). Keep numbers
and units (180g, 1/3 bowl) as-is.

STEP 4 - Before committing, validate the generated file with a script and fix any
failure rather than committing a bad file. Check all of:
- JSON parses; top-level keys are exactly meta, fixed, days, grocery
- meta.generatedAt is present and parses as an ISO 8601 timestamp (the app shows
  it as "Last generated"); set it with `date -Iseconds` at generation time
- days has mon,tue,wed,thu,fri,sat,sun in that order, each with
  breakfast,midmorning,lunch,dinner in that order
- each day's midmorning names one fruit and either almonds or walnut halves;
  walnut days number exactly 3 and are not adjacent
- every meal and fixed item (except coffee) has both "s" and "v"; every language
  object has all four of en, hi, kn, te, none empty
- all content checks below run on BOTH the "s" and "v" texts; "v" anchors are
  roughly half of "s" (a smaller gram/egg quantity must be present)
- grocery quantities cover BOTH people
- grocery has exactly the keys Proteins, Veg & Greens, Grains, Fruit, Pantry
- "Veg & Greens" has at most 11 lines (the week's 5 vegetables + staples)
- chicken meals <= 3; fish meals <= 2; both fish meals name the SAME fish and
  are >= 2 days apart
- within each day, no protein keyword (egg, paneer, tofu, soya, chicken, fish
  names, chana, rajma, lobia, moong, sattu) appears in more than one of the
  day's three meals (curd/yogurt/whey/milk exempt)
- no meal repeats within 2 days of its previous use
- no meal appears in the previous week's plan
- every breakfast contains 150g hung curd or 150g Greek yogurt
- every lunch and dinner names a protein anchor with a quantity (a "g" amount or
  an egg count in its text)
- every meal whose text contains "curry", "masala", "stew", or "gravy" also
  names a grain carrier (roti, or a bowl fraction of rice/quinoa) in the same meal
- the midmorning text names specific fruits and does not contain the word "rotate"
- no banned item appears: prawn, shellfish, sardine, mackerel, surmai, anchovy,
  tuna, salmon (except rawas/Indian salmon), barracuda, sheela, rohu, basa,
  organ meat, liver, sugar, honey, jaggery, juice, maida, deep-fried, white rice,
  brown bread, pancake, tikka, grill, grilled, bake, baked, pickle, papad, mango,
  banana, chikoo, lychee, jackfruit, dates, instant oats, flavoured oats

STEP 5 - Replace week.json at the repo root with the new file, commit on main, and
push. Commit message: "Weekly meal plan: <DD Mon> - <DD Mon YYYY>".
Do not commit anything other than week.json. If the push fails, say exactly what
the error was and leave the commit in place rather than retrying blindly.

STEP 6 - In your final message, confirm the commit hash and that the push
succeeded, then show the 7-day plan as an English table (rows = days, columns =
breakfast, lunch, dinner), plus a one-line note on what changed versus last week.

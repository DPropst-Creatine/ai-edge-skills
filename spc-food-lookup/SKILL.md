---
name: spc-food-lookup
description: Local JavaScript helper skill that searches a bundled food database, builds meals, estimates SPC Score, and returns plain-text paste-ready tracker blocks.
---

# SPC Food Lookup

## Purpose

Use this skill for known foods, restaurant/menu items, Panera-style meals, cruise foods, or meal comparisons where a database-based estimate is better than a pure visual guess.

SPC means Satiety Per Calorie. It is a custom 0-100 estimate based on protein, fiber, energy density, and hedonic load. It is not medical advice, not a precise nutrition label, not a glucose prediction, and not a proprietary scoring algorithm.

## Nonnegotiable Output Rule

Never show JSON to the user.

Always return a plain-text tracker block like this:

Paste Into Tracker:
Meal: [name]
Meal Type: [Breakfast / Lunch / Dinner / Snack / Dessert / Drink]
Calories: [number]
Protein: [number]
Carbs: [number]
Fat: [number]
Fiber: [number]
SPC: [number]
Glycemic: [Low / Moderate / High]
Notes: [brief note]

## Tool Use Rule

For database lookup, call `run_js` with script name `index.html`.

Do not calculate database totals yourself. The JavaScript tool calculates totals and returns the paste block.

## Preferred Actions

### Build meal from a natural description

Use this for most user requests:

```json
{"action":"meal","query":"half deli turkey sandwich, cup chicken noodle soup, apple side","meal_type":"Lunch","meal_name":"Half Turkey + Chicken Noodle Soup"}
```

### Search database

Use this when the user asks what options exist:

```json
{"action":"search","query":"turkey sandwich chicken noodle soup", "limit":8}
```

### Score custom macros

Use only when the user gives calories/macros:

```json
{"action":"score_custom","meal_name":"Custom meal","meal_type":"Lunch","calories":650,"protein":35,"carbs":65,"fat":30,"fiber":6,"glycemic":"Moderate","notes":"custom estimate"}
```

## Response After Tool Returns

Copy the tool's plain-text result. Keep your response short.

Suggested final format:

Best match or meal: [name]
Why it fits: [one sentence]

Paste Into Tracker:
[copy tool paste block exactly]

Notes:
- [one practical note]

## Rules

- Keep responses short.
- Do not output JSON to the user.
- Prefer database values when available.
- If the database match is weak, say it is a rough match.
- Do not invent exact official restaurant nutrition if it is not in the database.
- If the user asks for several options, rank by SPC, protein, and calorie fit.
- Use SPC language only.

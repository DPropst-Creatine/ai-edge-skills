---
name: spc-food-lookup
description: Local JavaScript helper skill that looks up common cruise, Panera, and generic foods from a bundled database, estimates SPC Score, and returns a paste-ready tracker block.
---

# SPC Food Lookup

## Purpose

Use this skill when the user wants a more consistent SPC estimate for a described food, menu item, or quick comparison.

SPC means Satiety Per Calorie. It is a custom 0-100 estimate based on protein, fiber, energy density, and hedonic load. It is not medical advice, not a precise nutrition label, not a glucose prediction, and not a proprietary scoring algorithm.

## What the JavaScript tool does

The JavaScript tool can:

1. Search a bundled local food database.
2. Return likely matches with calories, protein, carbs, fat, fiber, SPC, glycemic estimate, and notes.
3. Build a combined meal from multiple selected database entries.
4. Score a custom macro estimate using SPC rules.
5. Return a paste-ready block for Cruise Food Tracker v7 SPC.

The JavaScript tool cannot see images. If the user provides a food photo, first identify or describe the food, then call the tool with that description.

## Actions

### 1. Search food database

When the user asks about a food, menu item, or options, call `run_js` with script `index.html` and JSON:

```json
{"action":"search","query":"half turkey sandwich chicken noodle soup apple side","limit":8}
```

### 2. Build meal from known items

When the user describes a meal with several recognizable items, call:

```json
{"action":"build_meal","items":["half deli turkey sandwich","cup chicken noodle soup","apple side"],"meal_type":"Lunch","meal_name":"Half Turkey + Chicken Noodle Soup"}
```

### 3. Score custom macros

When the user or model provides macros but the item is not in the database, call:

```json
{"action":"score_custom","meal_name":"Custom meal","meal_type":"Lunch","calories":650,"protein":35,"carbs":65,"fat":30,"fiber":6,"glycemic":"Moderate","notes":"custom estimate"}
```

## Required response after the tool returns

Summarize briefly and include the exact paste block from the tool result.

Use this structure:

Best match or meal: [name]
Why it fits: [one sentence]

Paste Into Tracker:
[copy the tool's paste_block exactly]

Notes:
- [one or two practical notes]

## Rules

- Keep responses short.
- Prefer database values when available.
- If the database match is weak, say it is a rough match.
- Do not invent exact official nutrition for a restaurant item if it is not in the database.
- If the user asks for several options, rank by SPC, protein, and calorie fit.
- Use SPC language, not Hava-style language.

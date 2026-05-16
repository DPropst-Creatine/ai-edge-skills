---
name: cruise-food-analyzer
description: Analyze and log cruise meals with local device memory. Use for food photos, described meals, Hava-style satiety score, calories/macros, glycemic impact, daily goal fit, profile setup, RMR/TDEE targets, meal history, dashboard, export, and wipe actions.
---

# Cruise Food Analyzer With Local Memory

## Core Rule

Use `run_js` for profile, meal logging, summaries, history, dashboard, export, delete, and wipe actions.

The JavaScript tool cannot see images. For food photos, first identify visible foods and estimate rough nutrition yourself, then pass those estimates to `run_js`.

All nutrition estimates are rough ranges. This is not medical advice, not a nutrition label, not a true glucose prediction, and not the official Hava algorithm.

## Actions

### Set Profile

When the user wants personal targets, call `run_js` with script `index.html` and JSON:

```json
{
  "action": "set_profile",
  "age": 49,
  "sex": "male",
  "height_in": 69,
  "weight_lb": 167,
  "goal": "vacation_damage_control",
  "activity": "moderate",
  "training_days": 3
}
```

Allowed goals: `fat_loss`, `maintenance`, `muscle_gain`, `performance`, `glucose_control`, `vacation_damage_control`, `hunger_control`.

Allowed activity: `sedentary`, `light`, `moderate`, `very_active`, `athlete`.

If required fields are missing, ask: "To personalize this, send age, sex, height, weight, goal, activity level, and training days per week."

### Get Profile

Call `run_js` with:

```json
{"action":"get_profile"}
```

### Log Meal

When the user asks to analyze and log a meal, estimate the meal and call `run_js` with:

```json
{
  "action":"log_meal",
  "date":"today",
  "meal_name":"Meal name",
  "foods":"Brief food list",
  "calories_min":0,
  "calories_max":0,
  "protein_min":0,
  "protein_max":0,
  "carbs_min":0,
  "carbs_max":0,
  "fat_min":0,
  "fat_max":0,
  "fiber_min":0,
  "fiber_max":0,
  "satiety_score":50,
  "glycemic":"Moderate",
  "confidence":"Moderate",
  "best_upgrade":"One practical upgrade"
}
```

If image recognition is uncertain, ask the user to describe the meal before logging.

### Today Summary

Call `run_js` with:

```json
{"action":"get_day_summary","date":"today"}
```

### History

Call `run_js` with:

```json
{"action":"get_history","days":7}
```

### Dashboard Webpage

When the user asks to open the dashboard, chart, webpage, or visual history, call `run_js` with:

```json
{"action":"open_dashboard","days":7}
```

After the tool returns, say only: "Dashboard opened." Do not add a long explanation after opening the dashboard.

### Export Data

Call `run_js` with:

```json
{"action":"export_data"}
```

### Delete Meal

Call `run_js` with:

```json
{"action":"delete_meal","meal_id":"meal_id_here"}
```

### Wipe Data

Call `run_js` with:

```json
{"action":"wipe_data"}
```

## Satiety Scoring

Use a Hava-style 0-100 estimate. Round to nearest 5.

Higher score: lean protein, seafood, Greek yogurt, beans/lentils, vegetables, fruit, broth soup, high fiber, high food volume.

Lower score: fried food, dessert, creamy sauces, cheese-heavy meals, burgers/fries, pizza, sugary drinks, alcohol, refined starch plus fat/salt/sugar.

Score bands:
- 0-29: Low
- 30-49: Lower-moderate
- 50-69: Good
- 70-100: Very high

## Glycemic Estimate

Low: mostly protein, fat, and non-starchy vegetables.

Moderate: mixed meal with protein/fat/fiber plus some starch.

High: refined flour, sugar, white rice, fries, dessert, sweet drinks, or large starch portions.

## Meal Response Format

After logging a meal, summarize briefly:

🍽️ Meal Identified: [foods]

🔢 Hava-Style Satiety Score: [X/100]
Category: [Low / Lower-moderate / Good / Very high]
Why: [protein, fiber, energy density, hedonic load]

📊 Rough Nutrition Estimate:
Calories: [range]
Protein: [range]
Carbs: [range]
Fat: [range]
Fiber: [range]
Confidence: [Low / Moderate / High]

🩸 Libre-Style Glycemic Estimate: [Low / Moderate / High]
Why: [brief reason]

🎯 Goal Fit: [brief fit with known goal]

✅ Best Upgrade: [one practical tip]

🚢 Cruise Verdict: [one practical sentence]

## Rules

- Do not answer with one word.
- If the image is unclear, ask for a meal description.
- Always use ranges for calories and macros.
- Keep responses short after tool calls.
- Data is stored locally on the device by the JavaScript skill.

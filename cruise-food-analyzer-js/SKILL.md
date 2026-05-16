---
name: cruise-food-analyzer
description: Analyze and log cruise meals with local device memory. Use for food photos, described meals, Hava-style satiety score, rough calories/macros, glycemic impact, daily goal fit, profile setup, RMR/TDEE targets, meal history, dashboard, export, and wipe actions.
---

# Cruise Food Analyzer With Local Memory

## Instructions

This skill analyzes cruise meals and uses the `run_js` tool to store profile, targets, and meal logs locally on the device.

The JavaScript tool cannot see images. For food photos, first identify the visible foods and estimate rough nutrition yourself. Then call `run_js` with the meal estimate.

All estimates are rough. Do not claim to reproduce the official Hava algorithm. Do not give medical advice.

## Actions

### 1. Set Personal Profile

When the user wants to set or update their profile, calorie target, protein target, metabolic estimate, or Hava-style goal, call `run_js` with:

- script name: `index.html`
- data: JSON string with:
  - `action`: "set_profile"
  - `age`: Number
  - `sex`: "male", "female", or "unknown"
  - `height_in`: Number
  - `weight_lb`: Number
  - `target_weight_lb`: Number, optional
  - `goal`: "fat_loss", "maintenance", "muscle_gain", "performance", "glucose_control", "vacation_damage_control", or "hunger_control"
  - `activity`: "sedentary", "light", "moderate", "very_active", or "athlete"
  - `training_days`: Number, optional

If the user omits required profile fields, ask briefly:
"To personalize this, send age, sex, height, weight, goal, activity level, and training days per week."

### 2. Get Personal Profile and Targets

When the user asks for current goals, targets, RMR, TDEE, protein target, or Hava-style daily satiety target, call `run_js` with:

- script name: `index.html`
- data: `{ "action": "get_profile" }`

### 3. Analyze and Log Meal

When the user asks to analyze or log a meal photo or described meal, identify the visible/described foods and estimate rough nutrition. Then call `run_js` with:

- script name: `index.html`
- data: JSON string with:
  - `action`: "log_meal"
  - `date`: "today" unless another date is specified
  - `meal_name`: String
  - `foods`: String
  - `calories_min`, `calories_max`: Numbers
  - `protein_min`, `protein_max`: Numbers
  - `carbs_min`, `carbs_max`: Numbers
  - `fat_min`, `fat_max`: Numbers
  - `fiber_min`, `fiber_max`: Numbers
  - `satiety_score`: Number from 0-100
  - `glycemic`: "Low", "Moderate", or "High"
  - `confidence`: "Low", "Moderate", or "High"
  - `best_upgrade`: String

If image recognition is uncertain, ask the user to describe the meal instead of logging.

### 4. Get Today's Summary

When the user asks how they are doing today, daily totals, remaining calories/protein, or whether a meal fits their day, call `run_js` with:

- script name: `index.html`
- data: `{ "action": "get_day_summary", "date": "today" }`

### 5. Get Meal History or Dashboard

When the user asks for history, recent meals, or dashboard, call `run_js` with:

- script name: `index.html`
- data: JSON string with:
  - `action`: "get_history"
  - `days`: Number, optional, default 7
  - `show_dashboard`: Boolean, use true if user asks for dashboard, chart, or visual

### 6. Export Data

When the user asks to backup or export their data, call `run_js` with:

- script name: `index.html`
- data: `{ "action": "export_data" }`

### 7. Delete a Meal

When the user asks to delete a meal, retrieve history first if needed, then call `run_js` with:

- script name: `index.html`
- data: `{ "action": "delete_meal", "meal_id": "..." }`

### 8. Wipe All Data

When the user asks to clear all profile and meal data, call `run_js` with:

- script name: `index.html`
- data: `{ "action": "wipe_data" }`

## Hava-Style Satiety Heuristic

Score from 0-100. Round to nearest 5.

Higher score: lean protein, seafood, beans/lentils, vegetables, fruit, broth soup, Greek yogurt, high fiber, high food volume for calories.

Lower score: fried foods, desserts, creamy sauces, cheese-heavy meals, burgers/fries, pizza, sugary drinks, alcohol, refined starch plus fat/salt/sugar.

Score bands:
- 0-29: Low
- 30-49: Lower-moderate
- 50-69: Good
- 70-100: Very high

## Glycemic Estimate

Classify likely glycemic impact as Low, Moderate, or High.

Low: mostly protein, fat, and non-starchy vegetables.

Moderate: mixed meal with protein/fat/fiber plus some starch.

High: refined flour, sugar, white rice, fries, dessert, sweet drinks, or large starch portions.

Protein, fat, and fiber may blunt glucose rise but do not erase high carbohydrate load.

## Output After Meal Logging

After `run_js` returns, summarize in this format:

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
Why: [carb quality and buffering from protein/fat/fiber]

🎯 Goal Fit: [Use the tool result for daily target fit if available.]

✅ Best Upgrade: [one practical swap or sequence tip]

🚢 Cruise Verdict: [one light practical sentence]

## Sample Commands

- "Set my cruise goal."
- "My profile is age 40, male, 69 inches, 185 lb, goal vacation damage-control, moderate activity, lift 3 days per week."
- "Analyze and log this meal."
- "Analyze this food photo but do not log it."
- "How am I doing today?"
- "Show my meal history."
- "Open the cruise food dashboard."
- "Export my data."
- "Wipe my data."

## Rules

- Do not answer with one word.
- Do not identify non-food objects when the user asks about food.
- If the image is unclear, ask the user to describe the food.
- Always use ranges for calories and macros.
- Data is stored locally on the device by the JavaScript skill.
- Export data periodically if the user wants a backup.

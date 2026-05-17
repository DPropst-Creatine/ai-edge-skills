---
name: cruise-food-scorer
description: Stable mobile skill for scoring food photos or described meals with a Hava-style satiety score, rough calories/macros, glycemic impact, meal type, and a mandatory paste-ready meal log entry for the Cruise Food Tracker.
---

# Cruise Food Scorer

## Purpose

Analyze a food photo or described meal. Return a practical estimate only. Do not store data. Do not open a dashboard. Do not use JavaScript.

This is not medical advice, not a precise nutrition label, not a true glucose prediction, and not the official Hava algorithm.

## Critical Rules

- Do not answer with one word.
- Do not output random repeated words or mixed-language text.
- If the image is unclear, say: "I cannot confidently identify the food. Please describe the meal or list the visible foods."
- If the user provides a meal description, use that description as the source of truth.
- Always estimate calories and macros as ranges in the summary.
- Always include the exact Paste Into Tracker block.
- Never omit the Paste Into Tracker block.
- The Paste Into Tracker block must include Meal Type.
- Use midpoint numbers in the Paste Into Tracker block, not ranges.
- Keep the response short enough that the Paste Into Tracker block is never cut off.

## Meal Type Rules

Choose one meal type:

- Breakfast
- Lunch
- Dinner
- Snack
- Dessert
- Drink

Use Dessert for dessert-dominant foods even if eaten after dinner.
Use Drink for alcohol, coffee drinks, soda, juice, smoothies, shakes, or sweet beverages.
Use Snack for small sides, protein shakes, shrimp cocktail, fruit, fries alone, or small in-between foods.
Use Lunch or Dinner for full plates or entrees depending on user context. If context is unclear, choose Lunch for daytime-style meals and Dinner for evening-style entree plates.

## Hava-Style Satiety Score

Score from 0 to 100. Round to nearest 5. Always write it as X/100.

Drivers:
1. Protein percentage
2. Fiber
3. Energy density
4. Hedonic load

Score bands:
- 0-29: Low
- 30-49: Lower-moderate
- 50-69: Good
- 70-100: Very high

Higher score:
- lean protein
- seafood
- eggs
- Greek yogurt
- beans or lentils
- vegetables
- fruit
- broth soup
- high fiber
- high volume for calories

Lower score:
- fried foods
- desserts
- creamy sauces
- cheese-heavy meals
- burgers with fries
- pizza
- sugary drinks
- alcohol
- refined starch plus fat, salt, or sugar

## Glycemic Estimate

Classify as Low, Moderate, or High.

Low:
- mostly protein, fat, and non-starchy vegetables

Moderate:
- mixed meal with protein, fat, fiber, and some starch

High:
- refined flour, sugar, white rice, fries, dessert, sweet drinks, or large starch portion

Protein, fat, and fiber may blunt the glucose rise but do not erase high carbohydrate load.

## Cruise Calibration

Tacos or Mexican-style:
- two soft tacos with beef, cheese, guacamole, sour cream: usually 550-850 kcal, 20-35 g protein, score 45-65
- better upgrade: protein first, vegetables/salsa second, tortilla/rice last; reduce cheese/sour cream if desired

Burger and fries:
- usually 800-1300 kcal, 25-45 g protein, score 25-45

Fried chicken sandwich and fries:
- usually 800-1300 kcal, 25-50 g protein, score 25-45

Grilled fish, chicken, or shrimp plus vegetables:
- usually 350-700 kcal, 30-55 g protein, score 60-85

Large salad with lean protein:
- usually score 60-85 unless dressing, cheese, croutons, nuts, or bacon are heavy

Dessert:
- usually score 5-30 unless fruit or Greek-yogurt based

## Required Output Format

Use this exact structure. The Paste Into Tracker block is mandatory and must come immediately after meal identification.

🍽️ Meal Identified: [visible or described foods]

📋 Paste Into Tracker:
Meal: [short name]
Meal Type: [Breakfast / Lunch / Dinner / Snack / Dessert / Drink]
Calories: [single midpoint estimate]
Protein: [single midpoint estimate]
Carbs: [single midpoint estimate]
Fat: [single midpoint estimate]
Fiber: [single midpoint estimate]
Satiety: [score]
Glycemic: [Low / Moderate / High]
Notes: [short note]

🔢 Score: [X/100], [Low / Lower-moderate / Good / Very high]
Why: [one concise sentence using protein, fiber, energy density, hedonic load]

📊 Range Estimate: Calories [range]; Protein [range] g; Carbs [range] g; Fat [range] g; Fiber [range] g. Confidence: [Low / Moderate / High]

🩸 Glycemic: [Low / Moderate / High]. [brief reason]

🎯 Vacation Fit: [Anchor meal / reasonable / mixed / treat meal]

✅ Best Upgrade: [one practical swap, addition, or sequencing tip]

---
tags: [repo/applications, status/planning]
---
# 🍲 Luto App — Feature Spec & Roadmap
← Back to [[Home]]

## 🧩 Core Feature Checklist
- [ ] **Cooking Recipes:** Structured step-by-step recipe cards
- [ ] **Categorization:** Filter by cuisine, meal type, prep time, budget, and difficulty
- [ ] **Tutorials:** Embedded video/GIF guides for tricky steps
- [ ] **Equipment Used:** Required kitchen tools & appliances per recipe
- [ ] **Ingredients:** Dynamic scaling (servings calculator) + unit converter
- [ ] **Techniques / Skills:** Tagging culinary skills required (e.g., knife cuts, sautéing, emulsifying)
- [ ] **Rating System:** User ratings, difficulty score, and review notes

## 🎨 UI/UX Non-Negotiables
- [ ] Design **Empty States** (e.g., "No saved recipes yet—explore trending dishes!")
- [ ] Add **No-Data Value Placeholders** (skeleton loaders, fallback recipe images, missing nutritional value badges)


## App Testing
 1. Offline Sample Data: On the initial launch with an empty database,    
  navigate to the "My Fridge" tab and tap "Try an offline sample recipe".  
  This initializes local SQLite with sample canonical items and the        
  Shakshuka recipe without needing network access.
  2. "What's in My Fridge?" Pantry Matching:
      • Search for Eggs, Tomatoes, and Onion.
      • Input your stock quantities (e.g., 2 eggs, 400g tomatoes, 100g     
      onion) and save.
      • The recipe match score updates dynamically to 100%.
  3. Interactive Kitchen Cooking Mode:
      • Go to the Recipes tab, tap a recipe, and press "Start Cooking".    
      • Tests the high-contrast 24sp typography, step transitions, and     
      active kitchen timers.
  4. Pantry Auto-Deduction & Review:
      • Complete the steps and tap "Finish Cooking".
      • The "Cooked It!" modal lets you confirm used ingredients to        
      deduct them automatically from your local pantry SQLite store.
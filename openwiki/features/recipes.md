---
type: Feature Documentation
title: Recipe Management
description: Store and manage your own recipes with ingredients, instructions, and photos.
tags: [feature, recipes, cooking]
---
# Recipe Management

## Overview

The recipe feature allows users to store, manage, and discover recipes. Users can create their own recipes with ingredients, instructions, and photos, or discover recipes from external sources like TheMealDB. The system helps match pantry ingredients against recipes to determine what can be made and what ingredients are missing.

## Key Features

### Personal Recipes
- **Create**: Add new recipes with title, ingredients, instructions, and optional photo
- **Edit**: Modify existing recipes
- **Delete**: Remove recipes from your collection
- **View**: See recipe details including ingredient lists and instructions

### Recipe Discovery
- **TheMealDB Integration**: Access to 300+ recipes from TheMealDB
- **Browse**: Explore recipes by category, area, or ingredient
- **Search**: Find recipes by name or ingredient
- **Match**: Compare recipe ingredients against your pantry contents

### Ingredient Matching
- **Match Percentages**: See how well your pantry matches a recipe (Perfect Match, Almost There, More Needed)
- **Missing Ingredients**: Identify what you need to buy to make a recipe
- **One-Click Add**: Add missing ingredients directly to a grocery list

### Photo Support
- **Upload**: Add photos from your device
- **URL**: Reference images by URL
- **Display**: See photos in recipe views and lists

## User Interface

In the client application, users can:
- View a list of their personal recipes
- Create new recipes with a form
- Edit existing recipes
- Delete recipes
- Browse and search discovered recipes from TheMealDB
- See match percentages and missing ingredients for each recipe
- Add missing ingredients to a grocery list with one click
- View recipe details including ingredients, instructions, and photo

## API Endpoints

The API provides RESTful endpoints for recipe management:

### Personal Recipes
- `GET /recipes` - List all recipes for the authenticated user
- `POST /recipes` - Create a new recipe
- `GET /recipes/:id` - Retrieve a specific recipe
- `PATCH /recipes/:id` - Update a recipe
- `DELETE /recipes/:id` - Delete a recipe

### Recipe Discovery
- `GET /recipes/discover` - Get recipes from TheMealDB (with optional filters)
- `POST /recipes/match` - Match pantry ingredients against a recipe to get match percentage and missing ingredients

## Data Model

### Recipe Attributes
| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | integer | Primary key |
| `title` | string | Recipe name |
| `ingredients` | text | Ingredients list (format: JSON array or text) |
| `instructions` text | Step-by-step cooking instructions |
| `photo` | string | File path or URL to recipe image |
| `user_id` | foreign key | Reference to the owning user |
| `created_at` | timestamp | Record creation time |
| `updated_at` | timestamp | Last update time |

## Integration with Other Features

### Pantry
- Recipe discovery uses pantry contents to calculate match percentages
- Missing ingredients from recipes can be added to the pantry (if acquired) or to a grocery list

### Grocery Lists
- One-click button to add all missing ingredients from a recipe to a grocery list
- When adding missing ingredients, quantities and units are pre-filled from recipe data

### AI Assistant
- When configured, the AI can suggest recipes based on pantry contents
- The AI can also generate grocery lists from selected recipes

## Usage Examples

### Creating a Recipe
1. Navigate to the recipes section
2. Click "New Recipe"
3. Enter:
   - Title: "Spaghetti Bolognese"
   - Ingredients: "500g ground beef, 1 onion, 2 cloves garlic, 800g tomato sauce, 400g spaghetti"
   - Instructions: "1. Brown the beef. 2. Add onion and garlic. 3. Add tomato sauce and simmer. 4. Cook spaghetti separately. 5. Combine and serve."
   - Photo: (optional upload or URL)
4. Save the recipe

### Discovering Recipes
1. Go to the recipe discovery section
2. Browse or search for recipes (e.g., "chicken")
3. See how each recipe matches your pantry (e.g., "Perfect Match", "Almost There", "More Needed")
4. Click on a recipe to see details and missing ingredients
5. Click "Add Missing to Grocery List" to create a shopping list

### Using AI Suggestions
1. Ensure AI assistant is configured in settings
2. Click "Get Recipe Suggestions" in the pantry or recipe section
3. View suggested recipes based on what you have
4. Select a recipe to see details or generate a grocery list

## Best Practices

- Keep ingredient lists consistent (use standard units and names)
- Update recipes when you make changes or improvements
- Use photos to help visualize the dish
- Leverage recipe discovery to get inspiration based on what's in your pantry
- Regularly sync your pantry with actual kitchen contents for accurate matching

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System context
<!-- openwiki: broken internal link [./services/api.md] file "./services/api.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [API Service](./services/api.md) - Backend implementation
<!-- openwiki: broken internal link [./services/client.md] file "./services/client.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Client Service](./services/client.md) - Frontend implementation
<!-- openwiki: broken internal link [./features/pantry.md] file "./features/pantry.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - Pantry](./features/pantry.md) - Related feature
<!-- openwiki: broken internal link [./features/grocery-lists.md] file "./features/grocery-lists.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - Grocery Lists](./features/grocery-lists.md) - Related feature
<!-- openwiki: broken internal link [./features/ai-assistant.md] file "./features/ai-assistant.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - AI Assistant](./features/ai-assistant.md) - Related feature
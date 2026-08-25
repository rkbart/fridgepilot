---
type: Feature Documentation
title: Pantry Management
description: Track what ingredients you have on hand with quantities, units, categories, and expiry dates.
tags: [feature, pantry, inventory]
---
# Pantry Management

## Overview

The pantry feature allows users to track what ingredients they have available in their kitchen. Users can add items with specific quantities, units, categories, and expiry dates to help manage food inventory, reduce waste, and plan meals based on what's already on hand.

## Key Features

### Item Tracking
- **Name**: Ingredient name (case-insensitive unique per user)
- **Quantity**: Numerical amount (decimal for fractional quantities)
- **Unit**: Measurement unit (e.g., grams, pieces, cups, liters)
- **Category**: Organizational grouping (e.g., dairy, grains, spices, produce)
- **Expiry Date**: Optional date when item expires

### Duplicate Prevention
- The system prevents duplicate entries through case-insensitive name matching
- "Spaghetti" and "spaghetti" cannot coexist in the same user's pantry
- This ensures accurate inventory tracking without confusion over capitalization

### Organization
- Items can be grouped by category for easier browsing
- Sorting options by name, category, or expiry date
- Visual indicators for items nearing or past expiry date

## User Interface

In the client application, users can:
- View a list of all pantry items
- Add new items with a form
- Edit existing items
- Remove items from the pantry
- See visual cues for expiring items (typically highlighted in yellow/red)
- Filter or sort items by various criteria

## API Endpoints

The API provides RESTful endpoints for pantry management:

- `GET /pantry_items` - Retrieve all pantry items for the authenticated user
- `POST /pantry_items` - Add a new pantry item
- `GET /pantry_items/:id` - Retrieve a specific pantry item
- `PATCH /pantry_items/:id` - Update a pantry item
- `DELETE /pantry_items/:id` - Remove a pantry item

## Data Model

### Pantry Item Attributes
| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | integer | Primary key |
| `name` | string | Ingredient name (unique per user, case-insensitive) |
| `quantity` | decimal | Amount of the item |
| `unit` | string | Unit of measurement |
| `category` | string | Organizational category |
| `expiry_date` | date | Expiration date (nullable) |
| `user_id` | foreign key | Reference to the owning user |
| `created_at` | timestamp | Record creation time |
| `updated_at` | timestamp | Last update time |

## Integration with Other Features

### Recipe Discovery
- Pantry items are matched against recipe ingredients to determine what recipes can be made
- Match percentages are calculated based on available ingredients
- Missing ingredients can be added to grocery lists with one click

### Grocery Lists
- When viewing a recipe, users can add missing ingredients to a grocery list
- The system suggests quantities and units based on recipe requirements
- Items already in pantry are excluded from the missing ingredients calculation

### AI Assistant
- When configured, the AI analyzes pantry contents to suggest recipes
- The AI can generate grocery lists based on selected recipes and current pantry state

## Usage Examples

### Adding Items
1. Navigate to the pantry section
2. Click "Add Item"
3. Enter:
   - Name: "Chicken Breast"
   - Quantity: "500"
   - Unit: "grams"
   - Category: "Meat"
   - Expiry Date: (optional)
4. Save the item

### Checking Expiry
- Items approaching expiry (typically within 3 days) are highlighted
- Expired items are marked clearly
- Users can remove expired items or update quantities if still usable

### Meal Planning
1. View pantry contents
2. Identify items that need to be used soon
3. Use recipe discovery to find recipes that use those ingredients
4. Add missing ingredients to grocery list
5. Shop for missing items
6. Prepare the meal

## Best Practices

- Update quantities when using ingredients (don't just remove and re-add)
- Set realistic expiry dates based on food type and storage conditions
- Use categories to keep similar items together
- Regularly review and remove expired items
- Take advantage of case-insensitive matching to avoid duplicates

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System context
<!-- openwiki: broken internal link [./services/api.md] file "./services/api.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [API Service](./services/api.md) - Backend implementation
<!-- openwiki: broken internal link [./services/client.md] file "./services/client.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Client Service](./services/client.md) - Frontend implementation
<!-- openwiki: broken internal link [./features/recipes.md] file "./features/recipes.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - Recipes](./features/recipes.md) - Related feature
<!-- openwiki: broken internal link [./features/grocery-lists.md] file "./features/grocery-lists.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - Grocery Lists](./features/grocery-lists.md) - Related feature
<!-- openwiki: broken internal link [./features/ai-assistant.md] file "./features/ai-assistant.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - AI Assistant](./features/ai-assistant.md) - Related feature
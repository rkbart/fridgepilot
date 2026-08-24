---
type: Feature Documentation
title: Grocery Lists
description: Create and manage shopping lists with full CRUD operations, quantity/unit dropdowns, check-off status, and one-click addition of missing ingredients from recipes.
tags: [feature, grocery, shopping, list]
---
# Grocery Lists

## Overview

The grocery lists feature allows users to create and manage shopping lists for their grocery trips. Users can create multiple lists, add items with quantities and units, check off items as they are collected, and leverage one-click functionality to add missing ingredients from recipes with pre-filled quantities and units.

## Key Features

### Full CRUD Operations
- **Create**: Make new grocery lists (e.g., "Weekly Shop", "Party Supplies")
- **Read**: View lists and their items
- **Update**: Edit list names, item quantities, units, and check-off status
- **Delete**: Remove lists or individual items

### Item Management
- **Quantity**: Numerical amount with decimal support for fractional amounts
- **Unit**: Dropdown of common units (e.g., pieces, grams, cups, liters) or custom entry
- **Check-off**: Mark items as collected when shopping
- **Edit**: Modify quantities, units, or check-off status at any time

### Recipe Integration
- **One-click Add**: From a recipe view, add all missing ingredients to a grocery list
- **Pre-filled Data**: Quantities and units are automatically filled from recipe data when adding missing ingredients
- **Smart Matching**: System identifies which recipe ingredients you already have in your pantry

### Organization
- **Multiple Lists**: Maintain separate lists for different purposes or shopping trips
- **List Naming**: Descriptive names for easy identification
- **Item Ordering**: Maintains the order in which items were added (or can be sorted)

## User Interface

In the client application, users can:
- View a list of all grocery lists
- Create new lists with a name
- Open a list to view and manage its items
- Add items to a list with quantity and unit selectors
- Check off items as they are collected during shopping
- Edit item details (quantity, unit, name)
- Remove items from a list
- Delete entire lists
- From a recipe view, click "Add Missing to Grocery List" to populate a list with needed ingredients
- See which ingredients from a recipe are already in the pantry (reducing the list to only missing items)

## API Endpoints

The API provides RESTful endpoints for grocery list management:

### Grocery Lists
- `GET /grocery_lists` - List all grocery lists for the authenticated user
- `POST /grocery_lists` - Create a new grocery list
- `GET /grocery_lists/:id` - Retrieve a specific grocery list
- `PATCH /grocery_lists/:id` - Update a grocery list (e.g., change name)
- `DELETE /grocery_lists/:id` - Delete a grocery list

### Grocery List Items
- `POST /grocery_lists/:id/items` - Add an item to a grocery list
- `PATCH /grocery_lists/:id/items/:item_id` - Update a grocery list item (quantity, unit, checked status)
- `DELETE /grocery_lists/:id/items/:item_id` - Remove an item from a grocery list

## Data Model

### Grocery List Attributes
| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | integer | Primary key |
| `name` | string | Descriptive name for the list (e.g., "Weekly Groceries") |
| `user_id` | foreign key | Reference to the owning user |
| `created_at` | timestamp | Record creation time |
| `updated_at` | timestamp | Last update time |

### Grocery List Item Attributes
| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | integer | Primary key |
| `grocery_list_id` | foreign key | Reference to the parent grocery list |
| `name` | string | Item name (e.g., "Milk", "Bread") |
| `quantity` | decimal | Amount needed |
| `unit` | string | Unit of measurement |
| `checked` | boolean | Whether the item has been collected (default: false) |
| `created_at` | timestamp | Record creation time |
| `updated_at` | timestamp | Last update time |

## Integration with Other Features

### Pantry
- When adding missing ingredients from a recipe, the system checks the pantry to exclude items already available
- Quantities needed are calculated as: recipe quantity - pantry quantity (if pantry quantity is less than needed)

### Recipes
- From a recipe view, users can see missing ingredients based on current pantry contents
- One-click button adds all missing ingredients to a selected or new grocery list
- When adding items, quantities and units are pre-filled from the recipe data

### AI Assistant
- When configured, the AI can generate grocery lists from suggested recipes
- The generated list includes only missing ingredients based on current pantry state

## Usage Examples

### Creating a Shopping List
1. Navigate to the grocery lists section
2. Click "New List"
3. Enter a name: "Saturday Farmer's Market"
4. Save the list

### Adding Items to a List
1. Open your grocery list
2. Click "Add Item"
3. Enter:
   - Name: "Organic Strawberries"
   - Quantity: "2"
   - Unit: "baskets"
   - Checked: (leave unchecked)
4. Save the item

### Checking Off Items
1. While shopping, tap the checkbox next to each item as you place it in your cart
2. Checked items are visually marked (typically with a strikethrough or grayed out)
3. Uncheck items if you change your mind or need more

### Using Recipe Integration
1. View a recipe (either personal or discovered)
2. See the section showing missing ingredients based on your pantry
3. Click "Add Missing to Grocery List"
4. Choose an existing list or create a new one
5. The missing ingredients are added with quantities and units pre-filled from the recipe
6. Items you already have in sufficient quantity are omitted

### Managing Multiple Lists
1. Create separate lists for different needs: "Weekly Groceries", "Dinner Party", "Baking Supplies"
2. Switch between lists as needed
3. Archive or delete lists after use
4. Keep a running list of staples that you update regularly

## Best Practices

- Update quantities as you use items at home to keep your pantry accurate
- Use specific units (e.g., "grams" vs "cups") for better accuracy when following recipes
- Leverage the recipe integration to minimize forgotten ingredients
- Regularly review and clean up old lists
- Consider creating a "master" list of frequently bought items for quick reordering
- Check off items only when you're certain they're in your cart to avoid missing items

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System context
<!-- openwiki: broken internal link [./services/api.md] file "./services/api.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [API Service](./services/api.md) - Backend implementation
<!-- openwiki: broken internal link [./services/client.md] file "./services/client.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Client Service](./services/client.md) - Frontend implementation
<!-- openwiki: broken internal link [./features/pantry.md] file "./features/pantry.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - Pantry](./features/pantry.md) - Related feature (source of available items)
<!-- openwiki: broken internal link [./features/recipes.md] file "./features/recipes.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - Recipes](./features/recipes.md) - Related feature (source of missing ingredients)
<!-- openwiki: broken internal link [./features/ai-assistant.md] file "./features/ai-assistant.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - AI Assistant](./features/ai-assistant.md) - Related feature (can generate lists)
---
type: feature
title: Grocery List Page
description: The grocery list page for viewing and managing grocery lists and items in the FridgePilot frontend.
tags: ['frontend', 'page', 'grocery-list']
---
# Grocery List Page

The Grocery List page allows users to view, create, edit, delete their grocery lists, and manage items within each list. It is located at `src/pages/GroceryList.tsx`.

## Route
- Path: `/grocery-lists` (note: the actual route in the app may be `/grocery-lists` or similar; the component is named GroceryListPage)
- Component: `GroceryListPage`

## Functionality

The grocery list page displays a list of the user's grocery lists with the following features:

### Display
Each grocery list card shows:
- List name
- Item count and pending count
- Source indicator (Manual or AI-generated)
- Action buttons (rename, delete, expand/collapse)

### List Management
- **Create List**: Button to open a form for creating a new grocery list
- **Rename List**: Opens a modal to change the list name
- **Delete List**: Removes the list and all its items (with confirmation)
- **Expand/Collapse**: Toggle to show or hide the items within the list

### Item Management (within an expanded list)
Each item shows:
- Item name
- Quantity and unit (if provided)
- Source indicator (Manual or AI-suggested)
- Checkbox to mark item as completed/pending
- Action buttons (edit, delete)

### Item Actions
- **Toggle Status**: Click the checkbox to mark an item as completed (checked) or pending
- **Edit Item**: Opens a modal to modify the item's name, quantity, or unit
- **Delete Item**: Removes the item from the list
- **Add Item**: Button within an expanded list to add a new item to that list

### Add/Edit Forms
When adding or editing a grocery list or item, the form includes:
- **For Lists**: Name (text input, required)
- **For Items**: 
  - Name (text input, required)
  - Quantity (number input, optional, min 0)
  - Unit (select: count, grams, kg, ml, liters, cups, tablespoons, teaspoons, etc.)

### Validation
- List name is required
- Item name is required
- Quantity, if provided, must be a number >= 0
- Unit selection is optional but recommended when quantity is provided

## Implementation Details

### State Management
- Uses React state (hooks) to manage:
  - List of grocery lists
  - Search input and debounced query for filtering
  - Pagination state (current page, items per page)
  - Expanded list ID (to show/hide items)
  - UI states for adding, renaming, deleting lists, and editing items
  - Error messages

### API Interactions
All API calls are made through the `groceryLists` service (located in `src/services/api.ts`) and include the JWT token in the Authorization header.

#### Grocery Lists
- `GET /api/v1/grocery_lists`: Fetch user's grocery lists (with associated items via `includes(:grocery_items)`)
- `POST /api/v1/grocery_lists`: Create new grocery list
- `PUT /api/v1/grocery_lists/:id`: Update grocery list (e.g., rename)
- `DELETE /api/v1/grocery_lists/:id`: Delete grocery list

#### Grocery Items
- `POST /api/v1/grocery_lists/:list_id/items`: Add item to a list (nested under grocery_lists)
- `PUT /api/v1/grocery_lists/:list_id/items/:item_id`: Update item in a list
- `DELETE /api/v1/grocery_lists/:list_id/items/:item_id`: Delete item from a list

### UI Components
- Uses components from `src/components/`:
  - `EditModal.tsx`: Reusable modal for forms (list rename, item add/edit)
  - `ChevronActions.tsx`: Component to display an item with expandable actions (edit/delete)
  - Custom icons (PencilIcon, TrashIcon) for action buttons
- Styling from CSS modules or global styles (`index.css`, `App.css`)

### Features
- **Search**: Filter lists by name (case-insensitive, debounced)
- **Pagination**: Split lists into pages (10 per page) with navigation buttons
- **Optimistic Updates**: UI updates immediately on user action, then reconciles with API response
- **Source Badges**: Lists show "AI" or "Manual"; items show "AI" tag if AI-suggested
- **Completion Tracking**: Shows pending vs. total items in each list header

## Related Files
- `src/pages/GroceryList.tsx`: The page component
- `src/services/api.ts`: Service for making API calls (see `groceryLists` object)
- `src/components/EditModal.tsx`: Reusable modal for forms
- `src/components/ChevronActions.tsx`: Item display with action buttons
- `src/index.css`: Global styles
- `src/App.css`: Application-wide styles

## Testing
- Unit tests for the GroceryList component (if using a testing framework like Vitest or Jest)
- Integration tests simulating CRUD operations on lists and items
- Tests for search, pagination, and optimistic UI updates
- Tests for form validation and submission
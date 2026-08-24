---
type: feature
title: Pantry Page
description: The pantry page for viewing and managing pantry items in the FridgePilot frontend.
tags: ['frontend', 'page', 'pantry']
---
# Pantry Page

The Pantry page allows users to view, add, edit, and remove items from their virtual pantry. It is located at `src/pages/Pantry.tsx`.

## Route
- Path: `/pantry`
- Component: `Pantry`

## Functionality

The pantry page displays a list of the user's pantry items with the following features:

### Display
- Each pantry item shows:
  - Item name
  - Quantity and unit (e.g., "2 count", "500 grams")
  - Category (e.g., dairy, produce, meat)
  - Expiration date (if set)
  - Action buttons (edit, delete)

### Item Management
- **Add Item**: Opens a modal/form to add a new pantry item
- **Edit Item**: Opens a modal/form to modify an existing item
- **Delete Item**: Removes item from pantry (with confirmation)
- **Mark as Used**: Quick action to decrement quantity or remove item
- **Sorting**: Items can be sorted by name, category, or expiration date
- **Filtering**: Filter by category or search by name

### Add/Edit Form
When adding or editing a pantry item, the form includes:
- Name (text input, required)
- Quantity (number input, min 0)
- Unit (select: count, grams, kg, ml, liters, cups, tablespoons, teaspoons, etc.)
- Category (select: common food categories)
- Expires at (date input, optional)

### Validation
- Name is required
- Quantity must be a number >= 0
- Unit must be selected if quantity is provided
- Category selection is optional but recommended
- Expiration date, if provided, must be in the future

## Implementation Details

### State Management
- Uses React state (or context if implemented) to manage the list of pantry items
- Optimistic UI updates for immediate feedback
- Error handling for failed API requests

### API Interactions
- `GET /api/v1/pantry_items`: Fetch user's pantry items
- `POST /api/v1/pantry_items`: Create new pantry item
- `PUT /api/v1/pantry_items/:id`: Update pantry item
- `DELETE /api/v1/pantry_items/:id`: Delete pantry item

All requests include the JWT token in the Authorization header.

### UI Components
- Uses components from `src/components/` such as:
  - EditModal.tsx: For add/edit forms
  - ChevronActions.tsx: For item action buttons
  - Custom styling from CSS modules or global styles

## Related Files
- `src/pages/Pantry.tsx`: The page component
- `src/services/api.ts`: Service for making API calls to pantry endpoints
- `src/components/EditModal.tsx`: Reusable modal for forms
- `src/components/ChevronActions.tsx`: Action button component
- `src/index.css`: Global styles

## Testing
- Unit tests for the Pantry component (if using a testing framework)
- Integration tests simulating CRUD operations on pantry items
- Tests for form validation and submission
---
type: feature
title: Recipes Page
description: The recipes page for viewing and managing user recipes in the FridgePilot frontend.
tags: ['frontend', 'page', 'recipes']
---
# Recipes Page

The Recipes page allows users to view, add, edit, and remove their personal recipes. It is located at `src/pages/Recipes.tsx`.

## Route
- Path: `/recipes`
- Component: `Recipes`

## Functionality

The recipes page displays a list of the user's recipes with the following features:

### Display
Each recipe card shows:
- Recipe name
- Image (if uploaded)
- Preparation time, cooking time, servings
- Cuisine type
- Action buttons (view details, edit, delete)

### Recipe Management
- **Add Recipe**: Opens a form to create a new recipe
- **View Recipe**: Opens a detailed view of the recipe (ingredients, instructions, image)
- **Edit Recipe**: Opens a form to modify an existing recipe
- **Delete Recipe**: Removes recipe from user's collection (with confirmation)
- **Search**: Filter recipes by name or ingredients
- **Sort**: Sort by name, date created, preparation time, etc.

### Add/Edit Form
When adding or editing a recipe, the form includes:
- Name (text input, required)
- Image upload (optional)
- Ingredients (dynamic list, each with name, quantity, unit - at least one required)
- Instructions (dynamic list of steps, at least one required)
- Preparation time (number input, optional)
- Cooking time (number input, optional)
- Servings (number input, optional)
- Cuisine (select input, optional)
- Description (textarea, optional)

### Validation
- Name is required
- At least one ingredient is required (each ingredient requires name)
- At least one instruction step is required
- Image, if provided, must be an image file (JPEG, PNG, etc.) and within size limits
- Numeric fields must be valid numbers >= 0 when provided

## Implementation Details

### State Management
- Uses React state (or context if implemented) to manage the list of recipes
- Optimistic UI updates for immediate feedback
- Error handling for failed API requests

### API Interactions
- `GET /api/v1/recipes`: Fetch user's recipes
- `GET /api/v1/recipes/:id`: Fetch a specific recipe
- `POST /api/v1/recipes`: Create new recipe
- `PUT /api/v1/recipes/:id`: Update recipe
- `DELETE /api/v1/recipes/:id`: Delete recipe

All requests include the JWT token in the Authorization header.

### Image Upload
- Uses Active Storage on the backend to store recipe images
- Frontend sends multipart/form-data with the image file
- Image is displayed using the URL provided by the API

### UI Components
- Uses components from `src/components/` such as:
  - EditModal.tsx: For add/edit forms
  - Custom components for ingredient and instruction lists
  - ChevronActions.tsx: For item action buttons (in modals)
  - Styling from CSS modules or global styles

## Related Files
- `src/pages/Recipes.tsx`: The page component
- `src/services/api.ts`: Service for making API calls to recipe endpoints
- `src/components/EditModal.tsx`: Reusable modal for forms
- `src/components/ChevronActions.tsx`: Action button component
- `src/index.css`: Global styles

## Testing
- Unit tests for the Recipes component (if using a testing framework)
- Integration tests simulating CRUD operations on recipes
- Tests for form validation and submission
- Tests for image upload functionality
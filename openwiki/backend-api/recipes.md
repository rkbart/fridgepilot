---
type: feature
title: Recipes
description: CRUD endpoints for managing user's recipes.
---

# Recipes

The Recipes feature allows users to create, read, update, and delete their own recipes.

## Model
- **File**: `/backend/app/models/recipe.rb`
- **Attributes**:
  - `title`: String - Name of the recipe
  - `ingredients`: JSON/Array - Array of ingredient objects with name and optional quantity/unit
  - `instructions`: JSON/Array - Array of instruction strings
  - `image_url`: String - URL to recipe image (from external source or upload)
  - `user_id`: ForeignKey - References the user who owns this recipe

## Associations
- Belongs to `User`
- Has One ActiveStorage attachment for image (optional)

## Validations
- Presence of `title`, `ingredients`, `instructions`
- Custom validation `ingredients_must_be_valid`: ensures ingredients is an array where each element is a hash containing a `name` key
- Custom validation `instructions_must_be_valid`: ensures instructions is an array of strings

## API Endpoints
All endpoints are under `/api/v1/recipes` and require authentication.

### GET /api/v1/recipes
Retrieve all recipes for the current user.

**Response**:
```json
[
  {
    "id": 1,
    "title": "Chicken Stir Fry",
    "ingredients": [
      {"name": "chicken breast", "quantity": 500, "unit": "g"},
      {"name": "broccoli", "quantity": 2, "unit": "cups"},
      {"name": "soy sauce", "quantity": 3, "unit": "tbsp"}
    ],
    "instructions": [
      "Cut chicken into bite-sized pieces",
      "Stir fry chicken until browned",
      "Add vegetables and sauce",
      "Cook until tender"
    ],
    "image_url": null,
    "created_at": "2024-01-01T10:00:00Z",
    "updated_at": "2024-01-01T10:00:00Z"
  }
]
```
**Status**: 200 OK

### GET /api/v1/recipes/:id
Retrieve a specific recipe.

**Response**: Single recipe object (same format as above)
**Status**: 200 OK
**Error**: 404 if not found or doesn't belong to current user

### POST /api/v1/recipes
Create a new recipe.

**Request Body**:
```json
{
  "title": "Pasta Primavera",
  "ingredients": [
    {"name": "pasta", "quantity": 200, "unit": "g"},
    {"name": "mixed vegetables", "quantity": 2, "unit": "cups"},
    {"name": "olive oil", "quantity": 2, "unit": "tbsp"},
    {"name": "parmesan cheese", "quantity": 0.5, "unit": "cup"}
  ],
  "instructions": [
    "Cook pasta according to package directions",
    "Sauté vegetables in olive oil until tender",
    "Combine pasta and vegetables",
    "Top with parmesan cheese"
  ]
}
```
**Response**: Created recipe object
**Status**: 201 Created
**Error**: 422 Unprocessable Entity if validation fails

### PATCH/PUT /api/v1/recipes/:id
Update an existing recipe.

**Request Body**: Same format as POST (partial updates allowed)
**Response**: Updated recipe object
**Status**: 200 OK
**Error**: 
- 422 Unprocessable Entity if validation fails
- 404 if not found or doesn't belong to current user

### DELETE /api/v1/recipes/:id
Delete a recipe.

**Response**: Empty body
**Status**: 204 No Content
**Error**: 404 if not found or doesn't belong to current user

## Controller
- **File**: `/backend/app/controllers/api/v1/recipes_controller.rb`
- **Base Class**: `Api::V1::BaseController`
- **Actions**: index, show, create, update, destroy
- **Authentication**: Uses `authenticate_user!` before_action (inherited from BaseController)
- **Authorization**: Ensures users can only access their own recipes via scoping (`current_user.recipes`)

## Serialization
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#recipe_serializer] heading anchor "recipe_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- Uses `RecipeSerializer` (see [Serializers](/openwiki/backend-api/serializers.md#recipe_serializer))
- Includes all model attributes in the JSON response
- Formats ingredients and instructions as arrays for easy consumption by frontend

## Testing
Focused tests are in `/backend/spec/requests/recipes_controller_spec.rb`:
- Verify authentication is required for all endpoints
- Verify users can only access their own recipes
- Verify CRUD operations work correctly
- Verify validation errors are returned for invalid data (malformed ingredients/instructions)
- Verify image attachment handling (if implemented)

## Usage in Frontend
The frontend interacts with this API through the Recipe context and service layer:
- Loads recipes on app startup and when modified
- Provides UI for creating, viewing, editing, and deleting recipes
- Displays ingredients and instructions in user-friendly format
- Allows uploading images for recipes
- Enables searching/filtering recipes by title or ingredients
- Uses recipes for meal planning and grocery list generation

## Related Components
- [Authentication](/openwiki/backend-api/authentication.md) - Required for accessing this endpoint
- [Current User](/openwiki/backend-api/current-user.md) - User model that owns recipes
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#recipe_serializer] heading anchor "recipe_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- [Serializers](/openwiki/backend-api/serializers.md#recipe_serializer) - How recipes are formatted for API
- [Pantry Items](/openwiki/backend-api/pantry-items.md) - Ingredients from pantry can be used in recipes
- [Grocery Lists](/openwiki/backend-api/grocery-lists.md) - Recipes can generate grocery lists
- [AI Integration](/openwiki/backend-api/ai-integration.md) - AI can suggest recipes based on pantry items
- [Recipe Discovery](/openwiki/backend-api/recipe-discovery.md) - Finds recipes that match available pantry items
- [Active Storage](/openwiki/active-storage.md) - Handles recipe image uploads and storage
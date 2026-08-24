---
type: feature
title: Pantry Items
description: CRUD endpoints for managing user's pantry items.
---

# Pantry Items

The Pantry Items feature allows users to manage ingredients they have available in their pantry.

## Model
- **File**: `/backend/app/models/pantry_item.rb`
- **Attributes**:
  - `name`: String - Name of the ingredient
  - `quantity`: Decimal - Amount of the ingredient
  - `unit`: String - Unit of measurement (e.g., "g", "oz", "cups", "pieces")
  - `category`: String - Food category (e.g., "produce", "dairy", "grains")
  - `expires_at`: Date - Expiration date (optional)
  - `user_id`: ForeignKey - References the user who owns this pantry item

## Associations
- Belongs to `User`

## Validations
- Presence of `name`, `quantity`, `unit`
- `quantity` must be greater than 0

## API Endpoints
All endpoints are under `/api/v1/pantry_items` and require authentication.

### GET /api/v1/pantry_items
Retrieve all pantry items for the current user.

**Response**:
```json
[
  {
    "id": 1,
    "name": "Chicken Breast",
    "quantity": 500.0,
    "unit": "g",
    "category": "meat",
    "expires_at": "2024-01-15",
    "created_at": "2024-01-01T10:00:00Z",
    "updated_at": "2024-01-01T10:00:00Z"
  }
]
```
**Status**: 200 OK

### GET /api/v1/pantry_items/:id
Retrieve a specific pantry item.

**Response**: Single pantry item object (same format as above)
**Status**: 200 OK
**Error**: 404 if not found or doesn't belong to current user

### POST /api/v1/pantry_items
Create a new pantry item.

**Request Body**:
```json
{
  "name": "Tomatoes",
  "quantity": 4.0,
  "unit": "pieces",
  "category": "produce",
  "expires_at": "2024-01-10"
}
```
**Response**: Created pantry item object
**Status**: 201 Created
**Error**: 422 Unprocessable Entity if validation fails

### PATCH/PUT /api/v1/pantry_items/:id
Update an existing pantry item.

**Request Body**: Same format as POST (partial updates allowed)
**Response**: Updated pantry item object
**Status**: 200 OK
**Error**: 
- 422 Unprocessable Entity if validation fails
- 404 if not found or doesn't belong to current user

### DELETE /api/v1/pantry_items/:id
Delete a pantry item.

**Response**: Empty body
**Status**: 204 No Content
**Error**: 404 if not found or doesn't belong to current user

## Controller
- **File**: `/backend/app/controllers/api/v1/pantry_items_controller.rb`
- **Base Class**: `Api::V1::BaseController`
- **Actions**: index, show, create, update, destroy
- **Authentication**: Uses `authenticate_user!` before_action (inherited from BaseController)
- **Authorization**: Ensures users can only access their own pantry items via scoping (`current_user.pantry_items`)

## Serialization
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#pantry_item_serializer] heading anchor "pantry_item_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- Uses `PantryItemSerializer` (see [Serializers](/openwiki/backend-api/serializers.md#pantry_item_serializer))
- Includes all model attributes in the JSON response

## Testing
Focused tests are in `/backend/spec/requests/pantry_items_controller_spec.rb`:
- Verify authentication is required for all endpoints
- Verify users can only access their own pantry items
- Verify CRUD operations work correctly
- Verify validation errors are returned for invalid data
- Verify expiration date handling

## Usage in Frontend
The frontend interacts with this API through the Pantry context and service layer:
- Loads pantry items on app startup and when modified
- Provides UI for adding, editing, and removing pantry items
- Shows expiration warnings for items expiring soon
- Uses pantry items for recipe discovery and grocery list generation

## Related Components
- [Authentication](/openwiki/backend-api/authentication.md) - Required for accessing this endpoint
- [Current User](/openwiki/backend-api/current-user.md) - User model that owns pantry items
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#pantry_item_serializer] heading anchor "pantry_item_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- [Serializers](/openwiki/backend-api/serializers.md#pantry_item_serializer) - How pantry items are formatted for API
- [Recipe Discovery](/openwiki/backend-api/recipe-discovery.md) - Uses pantry items to find matching recipes
- [Grocery Lists](/openwiki/backend-api/grocery-lists.md) - Can generate grocery lists from pantry shortages
---
type: feature
title: Grocery Lists
description: CRUD endpoints for managing grocery lists and their items.
---

# Grocery Lists

The Grocery Lists feature allows users to create and manage shopping lists for ingredients they need to purchase.

## Models
### GroceryList
- **File**: `/backend/app/models/grocery_list.rb`
- **Attributes**:
  - `name`: String - Name/title of the grocery list (e.g., "Weekly Shopping", "Party Supplies")
  - `source`: String - Origin of the list: "manual" (user-created) or "ai_generated" (created by AI suggestion)
  - `user_id`: ForeignKey - References the user who owns this grocery list

### GroceryItem
- **File**: `/backend/app/models/grocery_item.rb`
- **Attributes**:
  - `name`: String - Name of the grocery item (e.g., "Milk", "Eggs")
  - `quantity`: Decimal - Amount needed (can be null for flexible quantities)
  - `unit`: String - Unit of measurement (e.g., "gallon", "dozen", "pieces")
  - `status`: String - Current state: "pending" (needs to be bought), "confirmed" (located in store), "checked" (purchased)
  - `source`: String - Origin: "manual" (user-added) or "ai_suggested" (added by AI)
  - `recipe_id`: ForeignKey (optional) - References recipe that requires this ingredient
  - `grocery_list_id`: ForeignKey - References the grocery list this item belongs to

## Associations
- GroceryList belongs_to User
- GroceryList has_many GroceryItems (dependent: :destroy)
- GroceryItem belongs_to GroceryList
- GroceryItem belongs_to Recipe (optional)

## Validations
### GroceryList
- `source` must be either "manual" or "ai_generated"
- Defaults to "manual" for new records

### GroceryItem
- Presence of `name`
- `status` must be one of: "pending", "confirmed", "checked"
- `source` must be one of: "manual", "ai_suggested"
- `quantity` must be >= 0 when present
- Defaults: status = "pending", source = "manual"

## API Endpoints
All endpoints are under `/api/v1` and require authentication.

### Grocery Lists

#### GET /api/v1/grocery_lists
Retrieve all grocery lists for the current user (includes grocery items).

**Response**:
```json
[
  {
    "id": 1,
    "name": "Weekly Shopping",
    "source": "manual",
    "created_at": "2024-01-01T10:00:00Z",
    "updated_at": "2024-01-01T10:00:00Z",
    "grocery_items": [
      {
        "id": 1,
        "name": "Milk",
        "quantity": 2.0,
        "unit": "gallon",
        "status": "pending",
        "source": "manual",
        "recipe_id": null,
        "created_at": "2024-01-01T10:00:00Z",
        "updated_at": "2024-01-01T10:00:00Z"
      }
    ]
  }
]
```
**Status**: 200 OK

#### GET /api/v1/grocery_lists/:id
Retrieve a specific grocery list with its items.

**Response**: Single grocery list object (same format as above)
**Status**: 200 OK
**Error**: 404 if not found or doesn't belong to current user

#### POST /api/v1/grocery_lists
Create a new grocery list.

**Request Body**:
```json
{
  "name": "Birthday Party Supplies",
  "source": "manual"
}
```
**Response**: Created grocery list object (without items initially)
**Status**: 201 Created
**Error**: 422 Unprocessable Entity if validation fails

#### PATCH/PUT /api/v1/grocery_lists/:id
Update an existing grocery list.

**Request Body**: Same format as POST (partial updates allowed)
**Response**: Updated grocery list object
**Status**: 200 OK
**Error**: 
- 422 Unprocessable Entity if validation fails
- 404 if not found or doesn't belong to current user

#### DELETE /api/v1/grocery_lists/:id
Delete a grocery list and all its items.

**Response**: Empty body
**Status**: 204 No Content
**Error**: 404 if not found or doesn't belong to current user

### Grocery Items (nested under grocery lists)

#### POST /api/v1/grocery_lists/:grocery_list_id/grocery_items
Add an item to a grocery list.

**Request Body**:
```json
{
  "name": "Eggs",
  "quantity": 1.0,
  "unit": "dozen",
  "status": "pending",
  "source": "manual"
}
```
**Response**: Created grocery item object
**Status**: 201 Created
**Error**: 
- 422 Unprocessable Entity if validation fails
- 404 if grocery list not found or doesn't belong to current user

#### PATCH/PUT /api/v1/grocery_items/:id
Update a grocery item (note: endpoint uses item ID directly).

**Request Body**: Same format as POST (partial updates allowed)
**Response**: Updated grocery item object
**Status**: 200 OK
**Error**: 
- 422 Unprocessable Entity if validation fails
- 404 if grocery item not found or doesn't belong to current user's list

#### DELETE /api/v1/grocery_items/:id
Remove an item from a grocery list.

**Response**: Empty body
**Status**: 204 No Content
**Error**: 404 if not found or doesn't belong to current user's list

## Controllers
### GroceryListsController
- **File**: `/backend/app/controllers/api/v1/grocery_lists_controller.rb`
- **Base Class**: `Api::V1::BaseController`
- **Actions**: index, show, create, update, destroy
- **Authentication**: Uses `authenticate_user!` before_action
- **Authorization**: Scopes all operations to `current_user.grocery_lists`

### GroceryItemsController
- **File**: `/backend/app/controllers/api/v1/grocery_items_controller.rb`
- **Base Class**: `Api::V1::BaseController`
- **Actions**: create, update, destroy
- **Authentication**: Uses `authenticate_user!` before_action
- **Authorization**: 
  - First finds grocery list belonging to current user (`current_user.grocery_lists.find(params[:grocery_list_id])`)
  - Then finds item within that list (`@grocery_list.grocery_items.find(params[:id])`)

## Serialization
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#grocery_list_serializer-and-grocery_item_serializer] heading anchor "grocery_list_serializer-and-grocery_item_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- Uses `GroceryListSerializer` and `GroceryItemSerializer` (see [Serializers](/openwiki/backend-api/serializers.md#grocery_list_serializer-and-grocery_item_serializer))
- GroceryListSerializer includes nested grocery_items array
- GroceryItemSerializer includes all model attributes

## Testing
Focused tests are in:
- `/backend/spec/requests/grocery_lists_controller_spec.rb`
- `/backend/spec/requests/grocery_items_controller_spec.rb`

Tests verify:
- Authentication is required for all endpoints
- Users can only access their own grocery lists and items
- CRUD operations work correctly for both lists and items
- Validation errors are returned for invalid data
- Proper handling of nested resources (items belong to lists)
- Source and status validation works correctly
- Cascade delete when removing a grocery list

## Usage in Frontend
The frontend interacts with this API through the Grocery context and service layer:
- Loads grocery lists on app startup and when modified
- Provides UI for creating, viewing, editing, and deleting grocery lists
- Allows adding, updating, checking off, and removing items from lists
- Supports switching between list views
- Enables marking items as "confirmed" when found in store and "checked" when purchased
- Generates lists from AI suggestions (source: ai_generated) or recipe ingredients
- Shares lists between users (potential future feature)

## Related Components
- [Authentication](/openwiki/backend-api/authentication.md) - Required for accessing this endpoint
- [Current User](/openwiki/backend-api/current-user.md) - User model that owns grocery lists
- [Serializers](/openwiki/backend-api/serializers.md) - How grocery lists and items are formatted for API
- [Recipes](/openwiki/backend-api/recipes.md) - Grocery items can be linked to recipes that require them
- [AI Integration](/openwiki/backend-api/ai-integration.md) - AI can generate grocery lists based on pantry shortages or meal plans
- [Recipe Discovery](/openwiki/backend-api/recipe-discovery.md) - Can generate shopping lists for discovered recipes
- [Pantry Items](/openwiki/backend-api/pantry-items.md) - Used to determine what ingredients are needed vs. what's available
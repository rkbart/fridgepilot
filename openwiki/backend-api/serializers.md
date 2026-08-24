---
type: feature
title: Serializers
description: ActiveModel Serializers used for formatting API responses.
---

# Serializers

FridgePilot uses custom serializer objects (not ActiveModel Serializers gem) to format JSON responses for API endpoints. Each model has a corresponding serializer that defines which attributes are included in the API response and how they are formatted.

## Overview
Serializers are simple Ruby objects that take a model instance and return a hash representing its JSON structure. They are used in controllers to prepare data for rendering.

## Serializer List

### PantryItemSerializer
- **File**: `/backend/app/serializers/pantry_item_serializer.rb`
- **Attributes**:
  - `id`: Integer
  - `name`: String
  - `quantity`: Float or nil
  - `unit`: String or nil
  - `category`: String or nil
  - `expires_at`: DateTime or nil (ISO 8601 string)
  - `created_at`: DateTime
  - `updated_at`: DateTime
- **Usage**: Used in `api/v1/pantry_items_controller` for index, show, create, update actions.

### RecipeSerializer
- **File**: `/backend/app/serializers/recipe_serializer.rb`
- **Attributes**:
  - `id`: Integer
  - `name`: String
  - `image_url`: String or nil (URL to attached image or external URL)
  - `ingredients`: Array (as stored in the database)
  - `instructions`: Array (as stored in the database)
  - `created_at`: DateTime
  - `updated_at`: DateTime
- **Note**: The `image_url` method handles both attached images (via Active Storage) and external URLs.
- **Usage**: Used in `api/v1/recipes_controller` for index, show, create, update actions.

### GroceryListSerializer
- **File**: `/backend/app/serializers/grocery_list_serializer.rb`
- **Attributes**:
  - `id`: Integer
  - `name`: String
  - `source`: String (`manual` or `ai_generated`)
  - `grocery_items`: Array of GroceryItemSerializer objects (nested)
  - `created_at`: DateTime
  - `updated_at`: DateTime
- **Usage**: Used in `api/v1/grocery_lists_controller` for index, show, create, update actions. Includes nested grocery items.

### GroceryItemSerializer
- **File**: `/backend/app/serializers/grocery_item_serializer.rb`
- **Attributes**:
  - `id`: Integer
  - `name`: String
  - `quantity`: Float or nil
  - `unit`: String or nil
  - `status`: String (`pending`, `confirmed`, or `checked`)
  - `source`: String (`manual` or `ai_suggested`)
  - `recipe_id`: Integer or nil (foreign key to recipes)
  - `created_at`: DateTime
  - `updated_at`: DateTime
- **Usage**: Used in `api/v1/grocery_items_controller` and nested in GroceryListSerializer.

### UserSerializer
- **File**: `/backend/app/serializers/user_serializer.rb`
- **Attributes**:
  - `id`: Integer
  - `email`: String
  - `name`: String or nil
  - `provider`: String or nil (for OAuth, if implemented)
  - `created_at`: DateTime
  - `updated_at`: DateTime
- **Note**: Does not include sensitive fields like `encrypted_password` or `ai_api_key`.
- **Usage**: Used in `api/v1/me_controller` and potentially other endpoints that return user data.

## How Serializers Are Used
In controllers, serializers are instantiated and their `serializable_hash` method is called to get the hash for JSON rendering.

Example from `api/v1/recipes_controller.rb`:
```ruby
def index
  recipes = current_user.recipes
  render json: recipes.map { |r| RecipeSerializer.new(r).serializable_hash }
end
```

## Testing
Serializer tests are typically included in the model or controller specs, verifying:
- All expected attributes are present
- Attribute types and formats are correct
- Relationships are serialized correctly (if applicable)
- Conditional logic (like image_url) works as expected

Example test for RecipeSerializer:
```ruby
describe RecipeSerializer do
  let(:recipe) { create(:recipe, name: "Test Recipe") }
  let(:serializer) { RecipeSerializer.new(recipe) }

  it "includes the expected attributes" do
    hash = serializer.serializable_hash
    expect(hash).to include(
      id: recipe.id,
      name: "Test Recipe",
      ingredients: recipe.ingredients,
      instructions: recipe.instructions
    )
  end
end
```

## Related Components
- [Models](/openwiki/backend-api/#models) - Each serializer corresponds to a model
- [Controllers](/openwiki/backend-api/) - Serializers are used in controller actions to format responses
<!-- openwiki: broken internal link [/openwiki/backend-api/api-contract.md] file "/openwiki/backend-api/api-contract.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [API Contract](/openwiki/backend-api/api-contract.md) - Serializers define the shape of API responses
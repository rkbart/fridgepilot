---
type: feature
title: AI Integration
description: Endpoints for AI-powered recipe suggestions and grocery list generation using NVIDIA NIM.
---

# AI Integration

The AI Integration feature provides endpoints for generating recipe suggestions based on pantry items and creating grocery lists from recipes using external AI services (primarily NVIDIA NIM).

## Overview
The system integrates with NVIDIA NIM (NVIDIA Inference Microservice) to provide AI-powered features:
1. **Recipe Suggestions**: Generate recipe ideas based on current pantry inventory
2. **Grocery List Generation**: Create optimized shopping lists for recipes by comparing ingredients with pantry items

AI API credentials are stored per-user in the database (encrypted) with a fallback to environment variable for development.

## Services
### AiService
- **File**: `/backend/app/services/ai_service.rb`
- **Responsibility**: Handles all communication with the external AI API
- **Configuration**:
  - Base URL: `https://integrate.api.nvidia.com/v1`
  - Default Model: `meta/llama-3.1-8b-instruct`
  - API Key: Retrieved from user's `ai_api_key` attribute or `NIM_API_KEY` environment variable
- **Key Methods**:
  - `suggest_recipes(pantry_items:)`: Gets recipe suggestions based on pantry items
  - `generate_grocery_list(recipe:, pantry_items:)`: Creates grocery list for a recipe
  - Error handling for rate limits (429), auth errors (401), and service errors
- **Custom Exceptions**:
  - `AiError`: Base exception for AI service errors
  - `AiRateLimitError`: Raised when API returns 429 (rate limited)
  - `AiAuthError`: Raised when API returns 401 (unauthorized)

### RecipeMatcher
- **File**: `/backend/app/services/recipe_matcher.rb`
- **Responsibility**: Matches pantry items with recipes from external sources
- **Used by**: Recipe Discovery feature (see below)
- **Note**: While this service interacts with external recipe databases (TheMealDB), it's not considered part of the core AI integration but rather a supporting service for discovery.

### TheMealDBClient
- **File**: `/backend/app/services/the_meal_db_client.rb`
- **Responsibility**: Client for accessing TheMealDB API (free recipe database)
- **Used by**: RecipeMatcher service
- **Note**: This is a separate integration from the AI service but works alongside it for recipe discovery.

## API Endpoints
All endpoints are under `/api/v1/ai` and require authentication.

### POST /api/v1/ai/suggest_recipes
Get recipe suggestions based on current pantry items.

**Request**: No body required (uses current user's pantry items)

**Response**:
```json
{
  "suggestions": [
    {
      "name": "Chicken Stir Fry",
      "ingredients": [
        {"name": "chicken breast", "quantity": 500, "unit": "g"},
        {"name": "broccoli", "quantity": 2, "unit": "cups"},
        {"name": "soy sauce", "quantity": 3, "unit": "tbsp"}
      ],
      "match_score": 0.8,
      "missing_ingredients": ["garlic", "ginger"]
    }
  ]
}
```
**Status**: 200 OK
**Error Responses**:
- 429 Too Many Requests: AI service rate limited
- 401 Unauthorized: Invalid/missing API key
- 503 Service Unavailable: AI service error
- 500 Internal Server Error: Unexpected error

**Notes**:
- Returns empty suggestions array with message if pantry is empty
- match_score is a float between 0.0 and 1.0 indicating ingredient coverage
- missing_ingredients lists items needed but not in pantry

### POST /api/v1/ai/generate_grocery_list
Generate a grocery list for a specific recipe by comparing with pantry items.

**Request Body**:
```json
{
  "recipe_id": 1
}
```
**Response**: Created grocery list object (see [Grocery Lists](/openwiki/backend-api/grocery-lists.md) for format)
**Status**: 201 Created
**Error Responses**:
- 404 Not Found: Recipe not found or doesn't belong to current user
- 429 Too Many Requests: AI service rate limited
- 401 Unauthorized: Invalid/missing API key
- 503 Service Unavailable: AI service error

**Process**:
1. Find the specified recipe belonging to current user
2. Get current user's pantry items
3. Call AI service to determine which recipe ingredients are needed (not in pantry)
4. Create a new grocery list with source "ai_generated"
5. Create grocery items for each needed ingredient, linking them to the recipe
6. Return the created grocery list

## Controller
- **File**: `/backend/app/controllers/api/v1/ai_controller.rb`
- **Base Class**: `Api::V1::BaseController`
- **Actions**: suggest_recipes, generate_grocery_list
- **Authentication**: Uses `authenticate_user!` before_action (inherited)
- **Error Handling**: Rescues AI service exceptions and returns appropriate HTTP status codes with JSON error responses

## Testing
Focused tests are in:
- `/backend/spec/services/ai_service_spec.rb`
- `/backend/spec/services/recipe_matcher_spec.rb`
- `/backend/spec/services/the_meal_db_client_spec.rb`
- `/backend/spec/requests/ai_controller_spec.rb`

Tests verify:
- Service correctly builds prompts for AI API
- Proper handling of API responses and errors
- Correct parsing of JSON responses from AI service
- Controller endpoints return correct data formats
- Authentication is required for endpoints
- Error handling for rate limits, auth failures, and service unavailability
- Grocery list generation correctly creates list and items
- Recipe matching with TheMealDB integration

## Usage in Frontend
The frontend interacts with this API through the AI service layer:
- Calls suggest_recipes when user wants recipe ideas based on pantry
- Displays suggested recipes with match scores and missing ingredients
- Allows user to select a suggested recipe and generate a grocery list
- Calls generate_grocery_list to create AI-powered shopping lists
- Shows loading states and error messages for AI service issues
- Stores user's AI API key in settings (see [User Settings](/openwiki/backend-api/user-settings.md))
- Indicates when lists are AI-generated vs manually created

## Related Components
- [Authentication](/openwiki/backend-api/authentication.md) - Required for accessing AI endpoints
- [Current User](/openwiki/backend-api/current-user.md) - Provides pantry items and stores AI API key
- [Pantry Items](/openwiki/backend-api/pantry-items.md) - Source data for recipe suggestions
- [Recipes](/openwiki/backend-api/recipes.md) - Recipes can be used to generate grocery lists
- [Grocery Lists](/openwiki/backend-api/grocery-lists.md) - Output of AI grocery list generation
- [Recipe Discovery](/openwiki/backend-api/recipe-discovery.md) - Uses external recipe DB alongside AI suggestions
- [User Settings](/openwiki/backend-api/user-settings.md) - Where users configure their AI API key
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#grocery_list_serializer] heading anchor "grocery_list_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- [Serializers](/openwiki/backend-api/serializers.md#grocery_list_serializer) - Formats AI-generated grocery lists for API
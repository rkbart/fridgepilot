---
type: feature
title: Recipe Discovery
description: Endpoint for discovering recipes based on pantry items using external recipe databases.
---

# Recipe Discovery

The Recipe Discovery feature allows users to find recipes that match their current pantry inventory by querying external recipe databases (TheMealDB) and ranking results based on ingredient overlap.

## Overview
Unlike AI-generated suggestions, this feature queries a curated recipe database (TheMealDB) to find existing recipes that use ingredients the user already has. It helps users discover what they can cook with their current pantry without needing to shop for additional ingredients.

## Services
### TheMealDbClient
- **File**: `/backend/app/services/the_meal_db_client.rb`
- **Responsibility**: Client for accessing TheMealDB API (www.themealdb.com), a free recipe database.
- **Key Methods**:
  - `search_by_ingredient(ingredient)`: Searches for recipes by ingredient
  - `lookup_recipe(id)`: Gets full recipe details by ID
- **Error Handling**: Raises `TheMealDbClient::ApiError` on API failures

### RecipeMatcher
- **File**: `/backend/app/services/recipe_matcher.rb`
- **Responsibility**: Compares recipe ingredients with user's pantry to calculate match percentages.
- **Used by**: Both AI Integration (indirectly) and Recipe Discovery
- **Note**: See [AI Integration](/openwiki/backend-api/ai-integration.md) for details on the matching algorithm.

## API Endpoint
### GET /api/v1/discover
Discover recipes based on provided ingredients (typically pantry items).

**Query Parameters**:
- `ingredients`: Array of ingredient names (can be repeated or provided as comma-separated string)
  - Example: `/api/v1/discover?ingredients[]=chicken&ingredients[]=broccoli`
  - Alternative: `/api/v1/discover?ingredients=chicken,broccoli`

**Response**:
```json
{
  "recipes": [
    {
      "id": "52771",
      "name": "Chicken Stir Fry",
      "image_url": "https://www.themealdb.com/images/media/milk/sytuqu1511803907.jpg",
      "category": "Chicken",
      "area": "Chinese",
      "instructions": "Cut the chicken into small pieces...",
      "youtube_url": "https://www.youtube.com/watch?v=...",
      "tags": "Chicken,Stir-Fry",
      "match_pct": 80,
      "total_ingredients": 5,
      "available_count": 4,
      "ingredients": [
        { "name": "chicken breast", "measure": "500g", "available": true },
        { "name": "broccoli", "measure": "2 cups", "available": true },
        { "name": "soy sauce", "measure": "3 tbsp", "available": true },
        { "name": "garlic", "measure": "2 cloves", "available": false },
        { "name": "ginger", "measure": "1 tsp", "available": false }
      ],
      "available": ["chicken breast", "broccoli", "soy sauce"],
      "missing": ["garlic", "ginger"]
    }
  ],
  "meta": {
    "total_searched": 12,
    "returned": 1,
    "query_ingredients": ["chicken", "broccoli"]
  }
}
```
**Status**: 200 OK
**Error Responses**:
- 502 Bad Gateway: TheMealDB API error (service unavailable or invalid response)

**Notes**:
- Returns empty recipes array if no ingredients provided
- `match_pct`: Percentage of recipe ingredients found in pantry (0-100)
- `total_ingredients`: Total number of ingredients in the recipe
- `available_count`: Number of recipe ingredients found in pantry
- Results are sorted by match percentage descending, then by available count descending
- Limited to 30 results (configurable via `MAX_RESULTS` constant)
- The system searches for each ingredient, collects recipe IDs, frequencies them (how many search terms matched), then fetches full details for top IDs
- Only recipes with at least one matching ingredient are returned

## Controller
- **File**: `/backend/app/controllers/api/v1/discover_controller.rb`
- **Base Class**: `Api::V1::BaseController`
- **Action**: index (only action)
- **Authentication**: Uses `authenticate_user!` before_action (inherited)
- **Process**:
  1. Parse and clean ingredients from params
  2. If no ingredients, return empty result
  3. Use TheMealDbClient to search for each ingredient, collecting recipe IDs with frequency counts
  4. Sort recipe IDs by frequency (most matched ingredients first)
  5. Fetch full recipe details for top IDs (up to 2x MAX_RESULTS to allow for filtering)
  6. Use RecipeMatcher to score each recipe against the user's pantry ingredients
  7. Sort results by match percentage (descending) and available count (descending)
  8. Limit to MAX_RESULTS (30)
  9. Return JSON with recipes and meta data

## Testing
Focused tests are in:
- `/backend/spec/services/the_meal_db_client_spec.rb`
- `/backend/spec/services/recipe_matcher_spec.rb`
- `/backend/spec/requests/discover_controller_spec.rb`

Tests verify:
- Client correctly builds URLs for TheMealDB API
- Proper handling of API responses and errors
- Recipe matching algorithm works correctly
- Controller returns correct data formats
- Authentication is required for the endpoint
- Error handling for TheMealDB API failures
- Ingredient normalization (case-insensitive, removing punctuation, etc.)
- Search frequency counting and sorting
- Result limiting and meta data correctness

## Usage in Frontend
The frontend interacts with this API through the discovery service layer:
- Typically called when user wants to see what they can cook with current pantry
- Sends current pantry items as ingredients parameter
- Displays discovered recipes with match percentages, images, and missing ingredients
- Allows user to select a recipe and either:
  - Save it to their personal recipes collection
  - Generate a grocery list for missing ingredients (via AI integration)
- Shows loading states and error messages for API issues
- May cache results or allow filtering by cuisine type, meal type, etc.

## Related Components
- [Authentication](/openwiki/backend-api/authentication.md) - Required for accessing this endpoint
- [Current User](/openwiki/backend-api/current-user.md) - Provides context but endpoint uses params ingredients (could be pantry items)
- [Pantry Items](/openwiki/backend-api/pantry-items.md) - Typical source of ingredients parameter
- [AI Integration](/openwiki/backend-api/ai-integration.md) - Can generate grocery lists for discovered recipes
- [Grocery Lists](/openwiki/backend-api/grocery-lists.md) - Where shopping lists for missing ingredients are stored
- [Recipes](/openwiki/backend-api/recipes.md) - User's personal recipes (separate from discovered recipes)
<!-- openwiki: broken internal link [/openwiki/backend-api/recipe-matcher.md#recipematcher] file "/openwiki/backend-api/recipe-matcher.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [RecipeMatcher](/openwiki/backend-api/recipe-matcher.md#recipematcher) - Shared service for calculating match percentages
- [TheMealDbClient](/openwiki/backend-api/recipe-discovery.md#themealdbclient) - External recipe database client
- [Serializers](/openwiki/backend-api/serializers.md) - Though this endpoint doesn't use serializers (builds JSON directly), it's part of the API consistency
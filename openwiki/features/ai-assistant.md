---
type: Feature Documentation
title: AI Assistant
description: Optional AI-powered features for recipe suggestions and grocery list generation using NVIDIA NIM.
tags: [feature, ai, assistant, nvidia, nim]
---
# AI Assistant

## Overview

The AI assistant is an optional feature that provides intelligent recipe suggestions based on your pantry contents and can generate grocery lists from recipes. It integrates with NVIDIA NIM (NVIDIA Inference Microservices) for AI capabilities. Users must configure the AI provider in their settings to use this feature.

## Key Features

### Recipe Suggestions
- **Pantry-Based**: Analyzes your current pantry items to suggest recipes you can make
- **Ingredient Matching**: Considers what you have available and suggests recipes with high match percentages
- **Discovery**: Helps users find new recipes based on existing ingredients
- **Reduces Waste**: Encourages using ingredients before they expire

### Grocery List Generation
- **From Recipes**: Takes a selected recipe and generates a grocery list for missing ingredients
- **Smart Calculation**: Factors in current pantry contents to only list what's needed
- **Pre-filled Quantities**: Uses recipe data to suggest appropriate amounts and units
- **One-Click**: Quickly create a shopping list from a recipe suggestion

### Configuration
- **Provider Selection**: Currently supports NVIDIA NIM
- **Per-User**: Each user can configure their own AI provider settings
- **Opt-In**: Feature is disabled until explicitly configured
- **Secure**: API keys and configuration stored securely

## User Interface

In the client application, when AI is configured, users can:
- Access AI features from the pantry or recipe sections
- Click "Get Recipe Suggestions" to view AI-generated recipe ideas
- See suggested recipes with match percentages and missing ingredients
- Select a recipe to view details or generate a grocery list
- Click "Generate Grocery List" from a recipe view to create a shopping list
- Access AI configuration in the settings page

## API Endpoints

The API provides endpoints for AI functionality (when configured):

### Recipe Suggestions
- `POST /ai/suggest-recipes` - Get recipe suggestions based on current pantry contents
  - Request: `{}` (uses authenticated user's pantry)
  - Response: Array of recipe suggestions with match data

### Grocery List Generation
- `POST /ai/generate-list` - Generate a grocery list from a recipe
  - Request: `{ "recipe_id": 123 }` or `{ "recipe": { ... } }`
  - Response: Grocery list items needed for the recipe

## Data Flow

1. **User Action**: User clicks "Get Recipe Suggestions" or "Generate Grocery List"
2. **Client Request**: SPA sends HTTPS request to API endpoint with JWT authentication
3. **API Processing**: 
   - Validates user authentication
   - For suggestions: Retrieves user's pantry items
   - For generation: Retrieves recipe and user's pantry
   - Calls AI provider (NVIDIA NIM) with appropriate prompt
4. **AI Processing**: NVIDIA NIM processes the request and returns results
5. **API Response**: Returns formatted data to client
6. **Client Display**: Shows suggestions or grocery list to user

## Configuration

### Settings Page
Users can configure the AI assistant in the settings:
- **Enable AI**: Toggle to turn on/off AI features
- **Provider**: Select NVIDIA NIM (current option)
- **API Key**: Enter the NVIDIA NIM API key
- **Model**: Select or configure the specific model to use (if applicable)
- **Region**: Optional region setting for the AI provider

### Security
- API keys are stored encrypted in the database
- All AI requests are proxied through the API (keys never exposed to client)
- Environment-based configuration for production deployments

## Integration with Other Features

### Pantry
- Recipe suggestions are based entirely on current pantry contents
- The AI considers quantities, categories, and expiry dates when making suggestions

### Recipes
- Can generate grocery lists from both personal recipes and discovered recipes
- Works with recipe matching to show what you need to buy

### Grocery Lists
- Generated lists can be saved as new grocery lists or added to existing ones
- Items are added with quantities and units pre-filled from recipe data

## Usage Examples

### Getting Recipe Suggestions
1. Ensure AI assistant is configured in Settings
2. Navigate to the Pantry or Recipes section
3. Click "Get Recipe Suggestions"
4. Wait for the AI to process (loading indicator shown)
5. View suggested recipes with:
   - Recipe title and image
   - Match percentage (e.g., "90% Match")
   - Missing ingredients list
   - Option to view full recipe or generate grocery list
6. Click on a suggestion to see more details

### Generating a Grocery List
1. From a recipe view (personal or discovered)
2. Click "Generate Grocery List"
3. Choose an existing grocery list or create a new one
4. The AI calculates missing ingredients based on your pantry
5. Items are added with quantities and units from the recipe
6. Navigate to your grocery list to see the generated items

## Best Practices

- Keep your pantry up-to-date for accurate suggestions
- Configure AI with a valid NVIDIA NIM API key
- Start with recipe suggestions to discover what you can make
- Use the grocery list generation to streamline shopping
- Review AI-generated lists for any preferences or substitutions
- Remember that AI is a supplement to your own judgment

## Technical Details

### AI Provider (NVIDIA NIM)
- **Service**: NVIDIA Inference Microservices
- **Capabilities**: Natural language processing for recipe understanding and generation
- **Integration**: API communicates over HTTPS with JSON payloads
- **Fallback**: If AI is unavailable, features gracefully degrade (show error or disable)

### Prompt Engineering
- Recipe suggestions: Prompt includes pantry items and requests recipe ideas
- List generation: Prompt includes recipe and pantry to calculate missing items
- Designed to work with culinary domain knowledge

### Rate Limits and Usage
- Subject to NVIDIA NIM service limits
- API may implement caching or queuing to manage usage
- Users should be aware of potential costs associated with AI provider usage

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System context
<!-- openwiki: broken internal link [./services/api.md] file "./services/api.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [API Service](./services/api.md) - Backend implementation (including AI endpoints)
<!-- openwiki: broken internal link [./services/client.md] file "./services/client.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Client Service](./services/client.md) - Frontend implementation
<!-- openwiki: broken internal link [./features/pantry.md] file "./features/pantry.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - Pantry](./features/pantry.md) - Source data for suggestions
<!-- openwiki: broken internal link [./features/recipes.md] file "./features/recipes.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - Recipes](./features/recipes.md) - Source for list generation
<!-- openwiki: broken internal link [./features/grocery-lists.md] file "./features/grocery-lists.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Features - Grocery Lists](./features/grocery-lists.md) - Destination for generated lists
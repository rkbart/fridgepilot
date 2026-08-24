---
type: reference
title: API Contract
description: Consolidated API reference for the FridgePilot backend endpoints.
tags: ['backend', 'api', 'reference']
---
# API Contract

This document provides a consolidated reference for all API endpoints in the FridgePilot backend. The API is versioned under `/api/v1` and uses JSON for request and response bodies.

## Base URL
- Development: `http://localhost:3001/api/v1`
- Production: Deployed to Google Cloud Run (see deployment documentation)

## Authentication
Most endpoints require authentication via JWT token in the `Authorization` header:
```
Authorization: Bearer <jwt_token>
```

Token is obtained through the Devise authentication endpoints.

## Endpoints

### Authentication
*Handled by Devise controllers at `/users/*`*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/users` | User registration | No |
| POST | `/users/sign_in` | User login | No |
| DELETE | `/users/sign_out` | User logout | Yes |
| POST | `/users/token/renew` | Renew JWT token | Yes |

### Current User
*Controller: `Api::V1::MeController`*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/v1/me` | Get current user profile | Yes |

**Response:**
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "provider": "email",
  "created_at": "2023-01-01T00:00:00Z",
  "updated_at": "2023-01-01T00:00:00Z"
}
```

### Pantry Items
*Controller: `Api::V1::PantryItemsController`*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/v1/pantry_items` | List all pantry items for current user | Yes |
| GET | `/api/v1/pantry_items/:id` | Get specific pantry item | Yes |
| POST | `/api/v1/pantry_items` | Create new pantry item | Yes |
| PUT | `/api/v1/pantry_items/:id` | Update pantry item | Yes |
| DELETE | `/api/v1/pantry_items/:id` | Delete pantry item | Yes |

**Pantry Item Object:**
```json
{
  "id": 1,
  "name": "eggs",
  "quantity": 12,
  "unit": "count",
  "category": "dairy",
  "expires_at": "2023-12-31",
  "created_at": "2023-01-01T00:00:00Z",
  "updated_at": "2023-01-01T00:00:00Z"
}
```

**Create/Update Parameters:**
- `name` (string, required)
- `quantity` (number, optional, >= 0)
- `unit` (string, optional)
- `category` (string, optional)
- `expires_at` (date string, optional)

### Recipes
*Controller: `Api::V1::RecipesController`*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/v1/recipes` | List all recipes for current user | Yes |
| GET | `/api/v1/recipes/:id` | Get specific recipe | Yes |
| POST | `/api/v1/recipes` | Create new recipe | Yes |
| PUT | `/api/v1/recipes/:id` | Update recipe | Yes |
| DELETE | `/api/v1/recipes/:id` | Delete recipe | Yes |

**Recipe Object:**
```json
{
  "id": 1,
  "name": "Scrambled Eggs",
  "image_url": "https://example.com/image.jpg",
  "ingredients": [
    {"name": "eggs", "quantity": 2, "unit": "count"},
    {"name": "milk", "quantity": 0.25, "unit": "cup"}
  ],
  "instructions": [
    "Beat eggs and milk together",
    "Cook in pan until set"
  ],
  "created_at": "2023-01-01T00:00:00Z",
  "updated_at": "2023-01-01T00:00:00Z"
}
```

**Create/Update Parameters:**
- `name` (string, required)
- `ingredients` (array of objects, required)
  - Each ingredient: `{ "name": string, "quantity": number, "unit": string }`
- `instructions` (array of strings, required)
- `image` (file, optional) - handled via Active Storage

### Grocery Lists
*Controller: `Api::V1::GroceryListsController`*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/v1/grocery_lists` | List all grocery lists for current user | Yes |
| GET | `/api/v1/grocery_lists/:id` | Get specific grocery list | Yes |
| POST | `/api/v1/grocery_lists` | Create new grocery list | Yes |
| PUT | `/api/v1/grocery_lists/:id` | Update grocery list | Yes |
| DELETE | `/api/v1/grocery_lists/:id` | Delete grocery list | Yes |

**Grocery List Object:**
```json
{
  "id": 1,
  "name": "Weekly Shopping",
  "source": "manual",
  "created_at": "2023-01-01T00:00:00Z",
  "updated_at": "2023-01-01T00:00:00Z",
  "grocery_items": [
    {
      "id": 1,
      "name": "bread",
      "quantity": 1,
      "unit": "loaf",
      "status": "pending",
      "source": "manual",
      "recipe_id": null,
      "created_at": "2023-01-01T00:00:00Z",
      "updated_at": "2023-01-01T00:00:00Z"
    }
  ]
}
```

**Create/Update Parameters:**
- `name` (string, required)
- `source` (string, required, one of: `manual`, `ai_generated`)

### Grocery Items
*Controller: `Api::V1::GroceryItemsController` (nested under grocery lists)*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/v1/grocery_lists/:list_id/items` | Add item to grocery list | Yes |
| PUT | `/api/v1/grocery_lists/:list_id/items/:id` | Update grocery item | Yes |
| DELETE | `/api/v1/grocery_lists/:list_id/items/:id` | Remove grocery item | Yes |

**Grocery Item Object:**
```json
{
  "id": 1,
  "name": "bread",
  "quantity": 1,
  "unit": "loaf",
  "status": "pending",
  "source": "manual",
  "recipe_id": null,
  "created_at": "2023-01-01T00:00:00Z",
  "updated_at": "2023-01-01T00:00:00Z"
}
```

**Create/Update Parameters:**
- `name` (string, required)
- `quantity` (number, optional, >= 0)
- `unit` (string, optional)
- `status` (string, optional, one of: `pending`, `confirmed`, `checked`)
- `source` (string, optional, one of: `manual`, `ai_suggested`)
- `recipe_id` (integer, optional)

### AI Integration
*Controller: `Api::V1::AiController`*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/v1/ai/suggest_recipes` | Get recipe suggestions based on pantry items | Yes |
| POST | `/api/v1/ai/generate_grocery_list` | Generate grocery list from recipe | Yes |

#### Suggest Recipes
**Request:**
```json
{}
```
*(No parameters needed - uses current user's pantry items)*

**Response:**
```json
{
  "suggestions": [
    {
      "name": "Scrambled Eggs",
      "ingredients": [
        {"name": "eggs", "quantity": 2, "unit": "count"},
        {"name": "milk", "quantity": 0.25, "unit": "cup"}
      ],
      "match_score": 0.8,
      "missing_ingredients": ["milk"]
    }
  ]
}
```

#### Generate Grocery List
**Request:**
```json
{
  "recipe_id": 1
}
```

**Response:** (Same as grocery list creation response)
```json
{
  "id": 1,
  "name": "Scrambled Eggs - Grocery List",
  "source": "ai_generated",
  "created_at": "2023-01-01T00:00:00Z",
  "updated_at": "2023-01-01T00:00:00Z",
  "grocery_items": [
    {
      "id": 1,
      "name": "milk",
      "quantity": 0.25,
      "unit": "cup",
      "status": "pending",
      "source": "ai_suggested",
      "recipe_id": 1,
      "created_at": "2023-01-01T00:00:00Z",
      "updated_at": "2023-01-01T00:00:00Z"
    }
  ]
}
```

### Recipe Discovery
*Controller: `Api::V1::DiscoverController`*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/v1/discover` | Discover recipes based on ingredients | Yes |

**Request:**
```json
{
  "ingredients": ["eggs", "milk", "cheese"]
}
```

**Response:**
```json
{
  "recipes": [
    {
      "id": 52771,
      "name": "Cheese Omelette",
      "image_url": "https://www.themealdb.com/images/media/meals/sytuqu1511804784.jpg",
      "category": "Breakfast",
      "area": "American",
      "instructions": "Beat eggs, milk, salt and pepper...",
      "youtube_url": "https://www.youtube.com/watch?v=1-QNeUl1ZJc",
      "tags": ["Breakfast","Eggs","Cheese"],
      "match_pct": 66.67,
      "total_ingredients": 3,
      "available_count": 2,
      "ingredients": [
        {"name": "egg", "measure": "2", "available": true},
        {"name": "milk", "measure": "2 tbsp", "available": true},
        {"name": "cheese", "measure": "2 tbsp", "available": false}
      ],
      "available": ["egg", "milk"],
      "missing": ["cheese"]
    }
  ],
  "meta": {
    "total_searched": 3,
    "returned": 1,
    "query_ingredients": ["eggs", "milk", "cheese"]
  }
}
```

### User Settings
*Controller: `Api::V1::SettingsController`*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/v1/settings` | Get user settings (AI configuration) | Yes |
| PUT | `/api/v1/settings` | Update user settings | Yes |

**Response (GET):**
```json
{
  "data": {
    "ai_api_key": "••••••••",
    "ai_api_endpoint": "https://integrate.api.nvidia.com/v1",
    "has_api_key": true
  }
}
```

**Request Parameters (PUT):**
- `ai_api_key` (string, optional)
- `ai_api_endpoint` (string, optional)

### Health Check
*Controller: `Rails::HealthController` (built-in)*

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/up` | Health check endpoint | No |

**Response:**
- `200 OK` if application is healthy
- `500 Internal Server Error` if application failed to boot

## Error Responses

Standard error response format:
```json
{
  "error": {
    "code": 422,
    "message": "Validation failed: Name can't be blank"
  }
}
```

Common HTTP status codes:
- `200`: Success
- `201`: Created
- `400`: Bad Request
- `401`: Unauthorized
- `403`: Forbidden
- `404`: Not Found
- `409`: Conflict
- `422`: Unprocessable Entity (validation errors)
- `429`: Too Many Requests (rate limiting)
- `500`: Internal Server Error
- `502`: Bad Gateway (external service error)
- `503`: Service Unavailable

## Rate Limiting
AI endpoints are subject to rate limiting from the NVIDIA NIM service. When rate limited, the API returns:
- Status: `429 Too Many Requests`
- Response includes: `{"error": {"code": 429, "message": "AI service rate limited. Please try again later."}, "rate_limited": true}`

## Versioning
The API is versioned in the URL path as `/api/v1`. Future versions would be `/api/v2`, etc.

## Related Documentation
- [Backend API Overview](/openwiki/backend-api/index.md)
- [Routing](/openwiki/backend-api/routing.md)
- [Serializers](/openwiki/backend-api/serializers.md)
- [Authentication](/openwiki/backend-api/authentication.md)
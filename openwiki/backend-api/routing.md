---
type: feature
title: API Routing
description: Structure and organization of the Rails 8 API endpoints.
---

# API Routing

This document describes the route structure of the FridgePilot Rails API, organized under the `/api/v1` namespace with Devise for authentication.

## Overview
The API uses Rails routing with namespacing and versioning. All API endpoints are under `/api/v1` and require JWT authentication (except for public endpoints if any existed, though all current endpoints require authentication).

## Route Structure
Defined in `/backend/config/routes.rb`, the API follows this structure:

### Authentication (Devise)
- **POST /users** - User registration (`users/registrations#create`)
- **POST /users/sign_in** - User login (`users/sessions#create`)
- **DELETE /users/sign_out** - User logout (`users/sessions#destroy`)
- **POST /users/token/renew** - JWT token renewal (`users/tokens#renew`)

### API Namespace (`/api/v1`)
All API endpoints are nested under the `api/v1` namespace.

#### Current User
- **GET /api/v1/me** - Retrieve current user profile (`api/v1/me#show`)

#### Recipes
- **GET /api/v1/recipes** - List all user recipes (`api/v1/recipes#index`)
- **GET /api/v1/recipes/:id** - Get specific recipe (`api/v1/recipes#show`)
- **POST /api/v1/recipes** - Create new recipe (`api/v1/recipes#create`)
- **PATCH/PUT /api/v1/recipes/:id** - Update recipe (`api/v1/recipes#update`)
- **DELETE /api/v1/recipes/:id** - Delete recipe (`api/v1/recipes#destroy`)

#### Pantry Items
- **GET /api/v1/pantry_items** - List all pantry items (`api/v1/pantry_items#index`)
- **GET /api/v1/pantry_items/:id** - Get specific pantry item (`api/v1/pantry_items#show`)
- **POST /api/v1/pantry_items** - Add new pantry item (`api/v1/pantry_items#create`)
- **PATCH/PUT /api/v1/pantry_items/:id** - Update pantry item (`api/v1/pantry_items#update`)
- **DELETE /api/v1/pantry_items/:id** - Remove pantry item (`api/v1/pantry_items#destroy`)

#### Grocery Lists
- **GET /api/v1/grocery_lists** - List all grocery lists (`api/v1/grocery_lists#index`)
- **GET /api/v1/grocery_lists/:id** - Get specific grocery list (`api/v1/grocery_lists#show`)
- **POST /api/v1/grocery_lists** - Create new grocery list (`api/v1/grocery_lists#create`)
- **PATCH/PUT /api/v1/grocery_lists/:id** - Update grocery list (`api/v1/grocery_lists#update`)
- **DELETE /api/v1/grocery_lists/:id** - Delete grocery list (`api/v1/grocery_lists#destroy`)

##### Nested Grocery Items
- **POST /api/v1/grocery_lists/:grocery_list_id/items** - Add item to grocery list (`api/v1/grocery_items#create`)
- **PATCH/PUT /api/v1/grocery_lists/:grocery_list_id/items/:id** - Update grocery item (`api/v1/grocery_items#update`)
- **DELETE /api/v1/grocery_lists/:grocery_list_id/items/:id** - Remove grocery item (`api/v1/grocery_items#destroy`)

#### AI Integration
- **POST /api/v1/ai/suggest_recipes** - Get recipe suggestions based on pantry (`api/v1/ai#suggest_recipes`)
- **POST /api/v1/ai/generate_grocery_list** - Generate grocery list for recipe (`api/v1/ai#generate_grocery_list`)

#### Recipe Discovery
- **POST /api/v1/discover** - Discover recipes based on ingredients (`api/v1/discover#index`)

#### User Settings
- **GET /api/v1/settings** - Get user settings (`api/v1/settings#show`)
- **PUT /api/v1/settings** - Update user settings (`api/v1/settings#update`)

#### Health Check
- **GET /up** - Application health check (`rails/health#show`)
  - Returns 200 if app boots with no exceptions, otherwise 500
  - Used by load balancers and uptime monitors

## Route Helpers
Rails provides helper methods for these routes:
- `me_api_v1_index_path` → `/api/v1/me`
- `recipes_api_v1_index_path` → `/api/v1/recipes`
- `pantry_items_api_v1_index_path` → `/api/v1/pantry_items`
- `grocery_lists_api_v1_index_path` → `/api/v1/grocery_lists`
- `api_v1_ai_suggest_recipes_path` → `/api/v1/ai/suggest_recipes`
- `api_v1_ai_generate_grocery_list_path` → `/api/v1/ai/generate_grocery_list`
- `api_v1_discover_index_path` → `/api/v1/discover`
- `api_v1_settings_path` → `/api/v1/settings`
- `rails_health_check_path` → `/up`

## Controller Organization
All API controllers inherit from `Api::V1::BaseController` which provides common functionality:
- Authentication via `authenticate_user!` (Devise)
- JSON response formatting
- Error handling patterns

### Base Controller (`/backend/app/controllers/api/v1/base_controller.rb`)
- Includes `ActionController::API` (lightweight controller for API-only apps)
- Before action: `authenticate_user!` (ensures JWT authentication)
- Provides `current_user` helper method
- Sets default JSON response format

## Versioning Strategy
The API uses URL versioning (`/api/v1/`) which allows:
- Backward compatibility when making breaking changes
- Clear indication of API version in client code
- Simple routing structure

## Security Considerations
- All API endpoints (except health check) require authentication
- Devise JWT handles token-based authentication
- JWT tokens are renewed via `/users/token/renew` endpoint
- Token revocation is handled via JTI (JWT ID) matching strategy
<!-- openwiki: broken internal link [/openwiki/configuration/cors-configuration.md] file "/openwiki/configuration/cors-configuration.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- CORS configuration allows frontend origins (see [CORS Configuration](/openwiki/configuration/cors-configuration.md))

## Testing Routes
Route tests are typically included in controller/request specs:
- Verify routes map to correct controllers/actions
- Test authentication requirements
- Validate parameter handling
- Check response formats

Example test patterns:
```ruby
# Routing spec
expect(get: "/api/v1/me").to route_to(
  controller: "api/v1/me",
  action: "show"
)

# Request spec with authentication
get "/api/v1/me", headers: auth_headers
expect(response).to have_http_status(:success)
```

## Frontend Integration
The frontend consumes these APIs through a service layer:
- Base URL typically set to `/api/v1` (relative) or full URL in production
- Authentication handled via JWT stored in localStorage/session
- Refresh token logic calls renewal endpoint when needed
- Error handling for 401 (unauthorized) responses
- All endpoints return JSON data structures

## Related Documentation
- [Authentication](/openwiki/backend-api/authentication.md) - Details on Devise + JWT flow
- [Controllers](/openwiki/backend-api/) - Individual controller documentation for each resource
- [Serializers](/openwiki/backend-api/serializers.md) - How models are serialized for API responses
- [Health Check](/openwiki/backend-api/health-check.md) - Application monitoring endpoint
- [Configuration](/openwiki/configuration/) - Environment variables and CORS settings
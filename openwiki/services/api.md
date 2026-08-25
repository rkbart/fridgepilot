---
type: Service Documentation
title: API Service
description: Ruby on Rails 8 backend providing JSON API for authentication, pantry, recipes, grocery lists, and AI integration.
tags: [backend, api, rails]
---
# API Service

## Overview

The FridgePilot API is a Ruby on Rails 8 application running in API-only mode that provides the backend functionality for the FridgePilot smart fridge companion. It handles authentication, pantry management, recipe storage, grocery lists, and optional AI features.

## Responsibilities

- **Authentication**: User signup, login, and JWT-based session management
- **Pantry Management**: CRUD operations for pantry items with duplicate prevention
- **Recipe Storage**: Store and manage user recipes with ingredients and instructions
- **Grocery Lists**: Create and manage shopping lists with item tracking
- **AI Integration**: Optional integration with NVIDIA NIM for recipe suggestions and list generation
- **Data Persistence**: Interact with PostgreSQL database via ActiveRecord

## Technology Stack

- **Framework**: Ruby on Rails 8 (API-only)
- **Authentication**: Devise + `devise-jwt` with JTI (JWT ID) revocation strategy
- **Database**: PostgreSQL with ActiveRecord ORM
- **API Format**: RESTful JSON endpoints
- **Deployment**: Google Cloud Run via `cloudbuild.yaml`

## Key Features

### Authentication System
- Email/password registration and login
- JWT tokens stored in HTTP-only cookies for security
- JTI revocation strategy enables immediate token invalidation on logout
- Password encryption and secure handling

### Pantry Management
- Track items with name, quantity, unit, category, and expiry date
- Case-insensitive duplicate prevention ("Spaghetti" and "spaghetti" cannot coexist)
- Category-based organization
- Expiry tracking for food waste reduction

### Recipe Functionality
- Store custom recipes with ingredients, instructions, and photos
- Support for file uploads or URL references
- Ingredient matching against pantry contents
- Recipe discovery from TheMealDB (300+ recipes) with match percentages

### Grocery Lists
- Full CRUD operations for list items
- Quantity and unit dropdowns
- Status check-off for completed items
- One-click addition of missing ingredients from recipes
- Pre-filled quantities and units from recipe data

### AI Assistant (Optional)
- Recipe suggestions based on current pantry contents
- Grocery list generation from selected recipes
- Configurable AI provider (NVIDIA NIM) per user in settings
- Opt-in feature requiring explicit user configuration

## API Endpoints

### Authentication
- `POST /auth/sign_up` - User registration
- `POST /auth/sign_in` - User login
- `DELETE /auth/sign_out` - User logout (token revocation)

### Pantry
- `GET /pantry_items` - List all pantry items
- `POST /pantry_items` - Add new pantry item
- `GET /pantry_items/:id` - Get specific pantry item
- `PATCH /pantry_items/:id` - Update pantry item
- `DELETE /pantry_items/:id` - Remove pantry item

### Recipes
- `GET /recipes` - List user recipes
- `POST /recipes` - Create new recipe
- `GET /recipes/:id` - Get specific recipe
- `PATCH /recipes/:id` - Update recipe
- `DELETE /recipes/:id` - Delete recipe
- `GET /recipes/discover` - Discover recipes from TheMealDB
- `POST /recipes/match` - Match pantry ingredients against recipes

### Grocery Lists
- `GET /grocery_lists` - List all grocery lists
- `POST /grocery_lists` - Create new grocery list
- `GET /grocery_lists/:id` - Get specific grocery list
- `PATCH /grocery_lists/:id` - Update grocery list
- `DELETE /grocery_lists/:id` - Delete grocery list
- `POST /grocery_lists/:id/items` - Add item to list
- `PATCH /grocery_lists/:id/items/:item_id` - Update list item
- `DELETE /grocery_lists/:id/items/:item_id` - Remove list item

### AI Features (When Configured)
- `POST /ai/suggest-recipes` - Get recipe suggestions from pantry
- `POST /ai/generate-list` - Generate grocery list from recipe

## Database Schema

### Users
- Devise standard fields (email, encrypted_password, etc.)
- JWT denylist via JTI strategy for token revocation

### Pantry Items
- `name`: string (unique per user, case-insensitive)
- `quantity`: decimal
- `unit`: string
- `category`: string
- `expiry_date`: date
- `user_id`: foreign key

### Recipes
- `title`: string
- `ingredients`: text (JSON or serialized)
- `instructions`: text
- `photo`: string (file path or URL)
- `user_id`: foreign key

### Grocery Lists
- `name`: string
- `user_id`: foreign key

### Grocery List Items
- `grocery_list_id`: foreign key
- `name`: string
- `quantity`: decimal
- `unit`: string
- `checked`: boolean (default: false)

## Security Considerations

- JWT authentication with HTTP-only cookie storage
- JTI revocation strategy prevents token replay attacks
- Strong parameter filtering for mass assignment protection
- CORS configured to allow only authorized origins
- Environment-based configuration for secrets
- Rate limiting on authentication endpoints
- Input validation and sanitization

## Deployment

The API is deployed to Google Cloud Run using the configuration in `cloudbuild.yaml`. The deployment process:
1. Builds Docker image with Rails application
2. Pushes to Google Container Registry
3. Deploys to Cloud Run service in `us-central1` region
4. Configured for scale-to-zero to minimize costs when idle

## Local Development

<!-- openwiki: broken internal link [./development/local.md] file "./development/local.md" does not exist. Fix the href or restore the target, then delete this comment. -->
See the [Development Workflow](./development/local.md) for instructions on running the API locally.

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System architecture context
<!-- openwiki: broken internal link [./deployment/cloud-run.md] file "./deployment/cloud-run.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Deployment - Cloud Run](./deployment/cloud-run.md) - Deployment specifics
<!-- openwiki: broken internal link [./services/client.md] file "./services/client.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Client Service](./services/client.md) - Frontend counterpart
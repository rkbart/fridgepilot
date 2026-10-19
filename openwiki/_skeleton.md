---
type: documentation-plan
title: FridgePilot Wiki Skeleton
description: Planned structure of the OpenWiki documentation for the FridgePilot repository.
---

# OpenWiki Skeleton for FridgePilot Repository

This file outlines the planned structure of the wiki. Each entry describes what will be documented in the corresponding wiki page.

## Table of Contents

1. [Quickstart](./quickstart.md)
2. [Architecture](#architecture)
3. [Backend API](#backend-api)
   - [Authentication](#authentication)
   - [Current User](#current-user)
   - [Pantry Items](#pantry-items)
   - [Recipes](#recipes)
   - [Grocery Lists](#grocery-lists)
   - [AI Integration](#ai-integration)
   - [Recipe Discovery](#recipe-discovery)
   - [User Settings](#user-settings)
   - [Routing](#routing)
   - [Serializers](#serializers)
   - [Health Check](#health-check)
   - [Background Jobs](#background-jobs)
4. [Frontend Client](#frontend-client)
<!-- openwiki: broken internal link [../frontend-client/overview.md] file "../frontend-client/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Overview](../frontend-client/overview.md)
<!-- openwiki: broken internal link [../frontend-client/components.md] file "../frontend-client/components.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Components](../frontend-client/components.md)

<!-- openwiki: broken internal link [../frontend-client/contexts.md] file "../frontend-client/contexts.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Contexts](../frontend-client/contexts.md)

<!-- openwiki: broken internal link [#frontend-pages] heading anchor "frontend-pages" does not exist in /openwiki/_skeleton.md. Fix the href or restore the target, then delete this comment. -->
   - [Pages](#frontend-pages)
<!-- openwiki: broken internal link [../frontend-client/services.md] file "../frontend-client/services.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Services](../frontend-client/services.md)
<!-- openwiki: broken internal link [../frontend-client/entry-points-and-styles.md] file "../frontend-client/entry-points-and-styles.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Entry Points and Styles](../frontend-client/entry-points-and-styles.md)
5. [Deployment](#deployment)
<!-- openwiki: broken internal link [../deployment/docker-compose.md] file "../deployment/docker-compose.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Docker Compose](../deployment/docker-compose.md)
<!-- openwiki: broken internal link [../deployment/cloud-run.md] file "../deployment/cloud-run.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Cloud Run (API)](../deployment/cloud-run.md)
<!-- openwiki: broken internal link [../deployment/vercel.md] file "../deployment/vercel.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Vercel (Client)](../deployment/vercel.md)
6. [Development Setup](#development-setup)
7. [Testing](#testing)
8. [Configuration](#configuration)
<!-- openwiki: broken internal link [../configuration/environment-variables.md] file "../configuration/environment-variables.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Environment Variables](../configuration/environment-variables.md)
<!-- openwiki: broken internal link [../configuration/cors-configuration.md] file "../configuration/cors-configuration.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [CORS Configuration](../configuration/cors-configuration.md)
<!-- openwiki: broken internal link [../configuration/devise-configuration.md] file "../configuration/devise-configuration.md" does not exist. Fix the href or restore the target, then delete this comment. -->
   - [Devise Configuration](../configuration/devise-configuration.md)
9. [Database Schema](#database-schema)
10. [API Contract](#api-contract)
11. [Active Storage](#active-storage)

## Detailed Page Descriptions

### Quickstart
- High-level introduction to the repository
- Links to every major concept
- Compact task-routing table from change intent to relevant page, source entrypoints, symbols, focused tests, and minimal validation commands

### Architecture
- System overview based on README architecture diagram
- Component responsibilities: Client (SPA), API (Rails), PostgreSQL, AI provider (NVIDIA NIM)
- Data flow and interaction patterns
- Deployment topology

### Backend API
#### Authentication
- Devise + JWT authentication flow
- Token renewal and revocation (JTI strategy)
- Controllers: users/registrations, users/sessions, users/tokens
- Related models: User
- Tests: authentication request specs

#### Current User
- GET /api/v1/me endpoint for retrieving current user
- Controller: api/v1/me_controller
- Related model: User
- Tests: me controller request specs

#### Pantry Items
- CRUD endpoints for pantry items
- Model: PantryItem (name, quantity, unit, category, expires_at)
- Controller: api/v1/pantry_items_controller
- Serializer: pantry_item_serializer
- Tests: pantry items request specs

#### Recipes
- CRUD endpoints for user recipes
- Model: Recipe (title, ingredients, instructions, image_url, etc.)
  - Custom validation: ingredients_must_be_valid ensures ingredients is array of objects with name
  - Custom validation: instructions_must_be_valid ensures instructions is array of strings
- Controller: api/v1/recipes_controller
- Serializer: recipe_serializer
- Tests: recipes request specs

#### Grocery Lists
- Grocery list CRUD and nested grocery items
- Models: GroceryList, GroceryItem
- Controllers: api/v1/grocery_lists_controller, api/v1/grocery_items_controller
- Serializers: grocery_list_serializer, grocery_item_serializer
- Tests: grocery lists and items request specs

#### AI Integration
- Endpoints for recipe suggestion and grocery list generation
- Controller: api/v1/ai_controller
- Services: ai_service, recipe_matcher, the_meal_db_client
- Tests: AI service specs, recipe matcher specs, TheMealDB client specs

#### Recipe Discovery
- Endpoint for discovering recipes based on pantry items
- Controller: api/v1/discover_controller
- Service: recipe_matcher (uses TheMealDB)
- Tests: discover controller specs

#### User Settings
- Endpoints for retrieving and updating user settings (AI provider configuration)
- Controller: api/v1/settings_controller
- Model: User (settings stored as serialized hash or JSONB)
- Tests: settings controller specs

#### Routing
- API route structure defined in config/routes.rb
- Namespace and versioning (api/v1)
- Custom endpoints: /users/token/renew, /up, /ai/*, /discover, /settings
- Resource routing for RESTful endpoints

#### Serializers
- ActiveModel Serializers for API responses
- Serializers for each model: pantry_item_serializer.rb, recipe_serializer.rb, grocery_list_serializer.rb, grocery_item_serializer.rb, user_serializer.rb
- How each model is serialized for API responses (attributes, relationships, formatting)

#### Health Check
- GET /up endpoint for health checking
- Returns 200 if app boots with no exceptions, otherwise 500
- Used by load balancers and uptime monitors
- Route: get "up" => "rails/health#show", as: :rails_health_check

#### Background Jobs
- ApplicationJob base class at /backend/app/jobs/application_job.rb
- Provides deadlock retry and deserialization error handling
- Foundation for any background jobs in the system

### Frontend Client
#### Overview
- React 19 + TypeScript SPA built with Vite
- State management (contexts)
- Routing (React Router)
- Styling approach

#### Components
- Reusable UI components (forms, buttons, cards, etc.)
- Location: src/components/
- Documentation of key components and their props

#### Contexts
- React context providers (Auth, Pantry, Recipe, Grocery, etc.)
- Location: src/contexts/
- How state is shared and manipulated

#### Pages
- Page components mapped to routes
- Location: src/pages/
- Description of each major page (Login, Signup, Pantry, Recipes, Grocery Lists, Settings, etc.)

#### Services
- API service layer (fetch API instance, endpoints)
- Location: src/services/
- Documentation of service functions and their usage

#### Entry Points and Styles
- App.tsx: Main application component with router and auth-aware layout
- main.tsx: Entry point that mounts the React app
- index.css: Global CSS styles
- assets/: Static assets (images, icons, etc.)
- styles/: Custom CSS design system and component styles

### Deployment
#### Docker Compose
- Local full-stack orchestration (Postgres + API + client behind nginx)
- docker-compose.yml breakdown
- Service definitions (db, backend, frontend)
- Volumes and network configuration

#### Cloud Run (API)

- Build steps, deployment configuration
- Environment variables and scaling

#### Vercel (Client)
- vercel.json in the client repo (referenced, but we note the frontend Dockerfile and nginx.conf for local/docker deployment)
- Note: The actual Vercel deployment is in the frontend repo, but we document the local/frontend setup

### Development Setup
- Prerequisites (Ruby, Node.js, PostgreSQL)
- Backend setup: bundle install, database creation, migrations, rails server
- Frontend setup: npm install, npm run dev
- Environment variables (.env.example)
- Running the full stack with docker compose

### Testing
- Backend: RSpec request specs and service specs
- Frontend: (if any tests exist, otherwise note absence)
- How to run tests (bundle exec rspec, npm test if configured)
- Test coverage focus areas

### Configuration
- Environment Variables
  - Backend: .env.example (database, Devise JWT, AI provider keys, etc.)
  - Frontend: .env.example (API URL, etc.)
- CORS Configuration
  - Config/initializers/cors.rb
  - allowed origins, methods, headers
- Devise Configuration
  - Config/initializers/devise.rb
  - JWT secret, expiration, revocation strategy

### Database Schema
- Tables: users, pantry_items, recipes, grocery_lists, grocery_items, active_storage tables (if used)
- Relationships: user has_many pantry_items, recipes, grocery_lists; grocery_list has_many grocery_items
- Important columns and indexes
- Migrations timeline (db/migrate/)

### API Contract
- Overview of all API endpoints under /api/v1
- Request/response examples for each endpoint
- Error response format
- Status codes
- Note: This can be generated from the code (routes, controllers, serializers) or maintained separately

### Active Storage
- Configuration for image uploads (for recipes)
- Model attachments (Recipe has_one_attached :image)
- Processed variants (if any)
- Storage service (local, cloud)
- Related migrations and schema
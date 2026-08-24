---
type: conceptual-guide
title: Architecture Overview
description: System overview based on README architecture diagram showing client, API, database, and AI provider interactions.
---

# FridgePilot Architecture

## System Overview

FridgePilot follows a client-server architecture with the following components:

```
┌─────────────┐   HTTPS/JSON   ┌──────────────┐   SQL   ┌─────────────┐
│  Client (SPA)│ ─────────────▶ │   API (Rails) │ ──────▶ │  PostgreSQL  │
│  Vite/React  │ ◀───────────── │  Puma/JWT    │ ◀────── │              │
└─────────────┘   JWT bearer    └──────────────┘         └─────────────┘
                                     │  HTTP
                                     ▼
                              ┌──────────────┐
                              │  AI provider │
                              │  (NVIDIA NIM)│
                              └──────────────┘
```

## Component Responsibilities

### Client (SPA)
- **Technology**: React 19 + TypeScript built with Vite
- **Responsibilities**:
  - User interface and user experience
  - State management via React Contexts
  - Routing with React Router
  - Communication with API via HTTP/JSON
  - Local storage for JWT token persistence
- **Location**: `/frontend/` directory

### API (Rails)
- **Technology**: Ruby on Rails 8 (API-only), PostgreSQL, Devise + JWT authentication
- **Responsibilities**:
  - Authentication and user management
  - Pantry items CRUD operations
  - Recipe management and storage
  - Grocery list management
  - AI integration for recipe suggestions and grocery list generation
  - Recipe discovery via TheMealDB integration
  - Health check endpoints
- **Location**: `/backend/` directory

### Database
- **Technology**: PostgreSQL
- **Responsibilities**:
  - Persistent storage for all application data
  - User accounts and authentication data
  - Pantry items with expiry tracking
  - User-created recipes
  - Grocery lists and items
  - User settings and preferences
- **Managed by**: Rails ActiveRecord migrations

### AI Provider (NVIDIA NIM)
- **Technology**: NVIDIA NIM (optional integration)
- **Responsibilities**:
  - Recipe suggestions based on pantry contents
  - Grocery list generation from recipes
  - Configured via user settings (NVIDIA NIM API key and URL)
- **Communication**: HTTP API calls from Rails backend

## Data Flow and Interaction Patterns

### User Authentication Flow
1. User signs in via `/users/sign_in` endpoint
2. API returns JWT token in `Authorization` header
3. Client stores token in localStorage
4. Subsequent requests include `Authorization: Bearer <token>` header
5. Token renewal via `/users/token/renew` before expiration
6. Token revocation on sign-out (JTI strategy)

### Typical User Workflow
1. User views pantry items (GET `/api/v1/pantry_items`)
2. User adds items to pantry (POST `/api/v1/pantry_items`)
3. User discovers recipes based on pantry (GET `/api/v1/discover`)
4. User saves or creates recipes (POST `/api/v1/recipes`)
5. User creates grocery lists from recipes or manually
6. Optional: User invokes AI for recipe suggestions or list generation

### AI Integration Flow
1. User configures AI provider in settings (NVIDIA NIM credentials)
2. User requests recipe suggestions from pantry contents
3. API calls `ai_service` → `recipe_matcher` → TheMealDB client
4. Or user requests grocery list generation from a recipe
5. API calls `ai_service` with recipe data to NVIDIA NIM
6. Results returned to client for display

## Deployment Topology

### Local Development
- Docker Compose orchestrates:
  - PostgreSQL database (host port 5432)
  - Rails API (port 3001)
  - React client served via nginx (port 5173)

### Production
- **API**: Deployed to Google Cloud Run via `cloudbuild.yaml`
  - Containerized Docker image
  - Scale-to-zero capability
  - Environment variables configured via Cloud Build
- **Client**: Deployed to Vercel (configuration in frontend repo)
- **Database**: External PostgreSQL (Neon or self-hosted)

## Key Characteristics

- **Stateless API**: All session state managed via JWT tokens
- **RESTful API**: Standard CRUD operations with proper HTTP verbs
- **CORS Enabled**: Configured for frontend-backend communication
- **Modular Services**: Business logic extracted to service objects
- **Background Job Ready**: ActiveJob base class implemented
- **File Storage**: Active Storage for recipe image attachments
---
type: Architecture Overview
title: FridgePilot Architecture
description: High-level system architecture showing the interaction between client, API, database, and optional AI provider.
tags: [architecture, system-design]
---
# FridgePilot Architecture

## System Overview

FridgePilot follows a classic three-tier architecture with a single-page application (SPA) frontend, a JSON API backend, and a PostgreSQL database, with optional integration to an AI provider for enhanced features.

<!-- openwiki: mermaid parse failed and this diagram was converted to a text fence so it does not break rendering. Fix the diagram source and restore the mermaid fence. Parser error: Parse error on line 2: ...graph Client[Client (SPA)] direc Expecting 'SQE', 'DOUBLECIRCLEEND', 'PE', '-)', 'STADIUMEND', 'SUBROUTINEEND', 'PIPE', 'CYLINDEREND', 'DIAMOND_STOP', 'TAGEND', 'TRAPEND', 'INVTRAPEND', 'UNICODE_TEXT', 'TEXT', 'TAGSTART', got 'PS' -->
```text
flowchart TB
    subgraph Client[Client (SPA)]
        direction TB
        Vite[Vite/React 19] --> Router[React Router]
        Router --> UI[User Interface]
    end
    
    subgraph API[API (Rails)]
        direction TB
        Rails[Ruby on Rails 8 API-only] --> Devise[Devise + devise-jwt]
        Devise --> JTI[JTI Revocation Strategy]
        JTI --> Controllers[API Controllers]
        Controllers --> Models[ActiveRecord Models]
    end
    
    subgraph DB[Database]
        PostgreSQL[PostgreSQL]
    end
    
    subgraph AI[AI Provider]
        NVIDIA[NVIDIA NIM]
    end
    
    Client -->|HTTPS/JSON Bearer Token| API
    API -->|SQL| DB
    API -->|HTTP| AI
    AI -->|HTTP| API
```

## Component Details

### Client (SPA)
- **Technology**: React 19 with TypeScript, built using Vite
- **Routing**: React Router for client-side navigation
- **Responsibilities**: 
  - User interface for pantry management, recipe browsing, and grocery lists
  - Authentication flow (login/register)
  - API communication via JSON over HTTPS
  - State management for UI components

### API (Rails)
- **Technology**: Ruby on Rails 8 (API-only mode)
- **Authentication**: Devise with `devise-jwt` using JTI (JWT ID) revocation strategy
- **Responsibilities**:
  - RESTful JSON API for all application data
  - User authentication and authorization
  - Business logic for pantry, recipes, and grocery lists
  - Integration with AI provider for optional features
  - Database interactions via ActiveRecord

### Database
- **Technology**: PostgreSQL
- **Responsibilities**:
  - Persistent storage for users, pantry items, recipes, grocery lists
  - JWT blacklist for token revocation (via JTI strategy)
  - Relationship modeling between entities

### AI Provider (Optional)
- **Technology**: NVIDIA NIM (NVIDIA Inference Microservices)
- **Responsibilities**:
  - Recipe suggestions based on pantry contents
  - Grocery list generation from recipes
  - Configurable per-user in settings

## Data Flow

1. **User Interaction**: User interacts with the React SPA interface
2. **API Request**: SPA sends HTTPS requests to Rails API with JWT bearer token
3. **Authentication**: API validates JWT using Devise/JTI strategy
4. **Processing**: API executes business logic, queries/modifies database
5. **AI Integration**: When needed, API makes HTTP calls to AI provider
6. **Response**: API returns JSON response to SPA
7. **UI Update**: SPA updates interface based on API response

## Deployment Characteristics

- **API**: Deployed to Google Cloud Run via `cloudbuild.yaml` (scale-to-zero capable)
- **Client**: Deployed to Vercel as static SPA
- **Database**: Can be hosted on Neon PostgreSQL or any PostgreSQL instance
- **Local Development**: Full stack可 run via `docker compose up`

## Security Considerations

- JWT-based authentication with HTTP-only cookie storage recommended
- JTI revocation strategy prevents token replay attacks
- CORS policies restrict API access to authorized origins
- Environment-based configuration for secrets (API keys, database URLs)
---
type: Service Documentation
title: Client Service
description: React 19 TypeScript SPA providing the user interface for FridgePilot, built with Vite and React Router.
tags: [frontend, client, react, spa]
---
# Client Service

## Overview

The FridgePilot client is a single-page application (SPA) built with React 19 and TypeScript, using Vite as the build tool and React Router for client-side navigation. It provides the user interface for interacting with the FridgePilot API to manage pantry items, recipes, grocery lists, and AI features.

## Responsibilities

- **User Interface**: Present all application features through an intuitive UI
- **Authentication Flow**: Handle user login, registration, and session management
- **Pantry Management**: Interface for adding, viewing, updating, and removing pantry items
- **Recipe Management**: Create, browse, and manage personal recipes
- **Grocery Lists**: Create and manage shopping lists with item tracking
- **Recipe Discovery**: Match pantry ingredients against external recipe databases
- **AI Integration**: Interface for optional AI-powered recipe suggestions and list generation
- **State Management**: Manage client-side state and synchronize with API
- **Navigation**: Client-side routing between different application views

## Technology Stack

- **Framework**: React 19 with TypeScript
- **Build Tool**: Vite
- **Routing**: React Router
- **Styling**: CSS/modules or similar (inferred from typical React setup)
- **API Communication**: Fetch/AJAX for HTTPS JSON API calls
- **Deployment**: Static SPA deployed to Vercel

## Key Features

### Authentication
- Login and registration forms
- Secure storage of authentication tokens (HTTP-only cookies via API)
- Protected routes requiring authentication
- Logout functionality that clears session

### Pantry Interface
- Add items with name, quantity, unit, category, and expiry date
- View pantry items in organized lists (by category, expiry, etc.)
- Edit existing pantry items
- Remove items from pantry
- Visual indicators for expiring items
- Case-insensitive duplicate prevention (handled by API, reflected in UI)

### Recipe Management
- Create recipes with title, ingredients, instructions, and photo
- Browse personal recipe collection
- Edit existing recipes
- Delete recipes
- Upload recipe photos or provide image URLs
- View recipe details including ingredient lists

### Recipe Discovery
- Search and browse recipes from TheMealDB (300+ recipes)
- Match pantry ingredients against discovered recipes
- Display match percentages (Perfect Match / Almost There / More Needed)
- One-click addition of missing ingredients to grocery list
- Ability to save discovered recipes to personal collection

### Grocery Lists
- Create multiple grocery lists (e.g., weekly, special events)
- Add items to lists with quantity and unit
- Check off items as they are collected
- Edit item quantities and units
- Remove items from lists
- Delete entire lists
- One-click addition of all missing ingredients from a recipe
- Pre-filled quantities and units from recipe data when adding missing ingredients

### AI Assistant (When Configured)
- Button to get recipe suggestions based on current pantry contents
- Interface to view and select suggested recipes
- Button to generate a grocery list from a selected recipe
- Settings page to configure AI provider (NVIDIA NIM)
- Opt-in nature requiring explicit user setup

## API Communication

The client communicates with the FridgePilot API over HTTPS using JSON format:

- **Authentication**: JWT bearer tokens stored in HTTP-only cookies (managed by API, sent automatically)
- **Endpoints**: RESTful JSON API under `/` namespace (e.g., `/pantry_items`, `/recipes`)
- **Error Handling**: Standard HTTP status codes with JSON error responses
- **Loading States**: UI shows loading indicators during API requests
- **Error Display**: User-friendly error messages for failed requests

## Routing

Client-side routing implemented with React Router:

- `/` - Home/dashboard (pantry overview)
- `/login` - Authentication page
- `/register` - Registration page
- `/pantry` - Pantry management view
- `/recipes` - Recipe collection view
- `/recipes/new` - Recipe creation form
- `/recipes/:id` - Individual recipe view/edit
- `/grocery-lists` - Grocery lists overview
- `/grocery-lists/new` - New grocery list form
- `/grocery-lists/:id` - Individual grocery list view/edit
- `/settings` - User settings including AI configuration
- `/recipe-discovery` - Browse and match recipes from TheMealDB

## State Management

- React hooks (useState, useEffect) for component-level state
- Context API or similar for global state (inferred)
- State synchronization with API data
- Optimistic UI updates where appropriate
- Caching of frequently accessed data

## Security Considerations

- Reliance on API for authentication and authorization
- No sensitive data stored in localStorage or sessionStorage (tokens in HTTP-only cookies)
- Input validation and sanitization on user-submitted data
- Protection against common web vulnerabilities (XSS, CSRF) via API and React best practices
- Environment-based configuration for API URLs

## Deployment

The client is deployed as a static SPA to Vercel:

- Built with `vite build` producing static assets
- Served via Vercel's edge network
- SPA routing handled by Vercel's rewrite configuration (see `vercel.json` in client repo)
- Environment variables for API URL and other configuration
- Automatic deployments from Git pushes

## Local Development

<!-- openwiki: broken internal link [./development/local.md] file "./development/local.md" does not exist. Fix the href or restore the target, then delete this comment. -->
See the [Development Workflow](./development/local.md) for instructions on running the client locally.

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System architecture context
<!-- openwiki: broken internal link [./deployment/docker-compose.md] file "./deployment/docker-compose.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Deployment - Local Docker Compose](./deployment/docker-compose.md) - Local full-stack development
<!-- openwiki: broken internal link [./services/api.md] file "./services/api.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [API Service](./services/api.md) - Backend counterpart
- [Features](./features/) - Detailed feature documentation
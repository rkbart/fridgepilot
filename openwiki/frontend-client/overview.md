---
type: overview
title: Frontend Client Overview
description: High-level overview of the React/Vite frontend application, including architecture, state management, and styling approach.
---

# Frontend Client Overview

The FridgePilot frontend is a single-page application (SPA) built with React 19, TypeScript, and Vite. It consumes the Rails API to provide a user interface for managing pantry items, recipes, grocery lists, and AI-powered features.

## Technology Stack
- **Framework**: React 19 with TypeScript
- **Build Tool**: Vite (for fast development and optimized builds)
- **State Management**: React Context API (for global state)
- **Routing**: React Router v6 (for client-side navigation)
<!-- openwiki: broken internal link [/openwiki/frontend-client/entry-points-and-styles.md] file "/openwiki/frontend-client/entry-points-and-styles.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- **Styling**: Custom CSS with BEM-like conventions (see [Entry Points and Styles](/openwiki/frontend-client/entry-points-and-styles.md))
- **API Communication**: Fetch API wrapped in a service layer (see [Services](/openwiki/frontend-client/services.md))

## Architecture
The frontend follows a component-based architecture with separation of concerns:
- **Components**: Reusable UI building blocks (buttons, forms, cards, etc.)
- **Contexts**: Global state management for authentication, pantry, recipes, grocery lists, etc.
- **Pages**: Route-specific components that assemble the UI for each view
- **Services**: API service layer that handles communication with the backend
- **Assets**: Static assets (images, icons, etc.)
- **Styles**: Global and component-specific CSS

## Data Flow
1. User interacts with UI components
2. Component state is updated locally or via context
3. Context state is persisted to backend via service layer
4. Backend responds with updated data
5. Service layer updates context state
6. Components re-render with new data

## Authentication Flow
- Users log in via the Login page
- Upon successful authentication, JWT is stored in localStorage
- The Auth context provides the current user and login/logout functions
- Protected routes use the RequireAuth component to redirect unauthenticated users
- API requests include the JWT in the Authorization header
- Token renewal is attempted automatically when a 401 response is received

## Key Features
- Pantry management (add, edit, remove items)
- Recipe creation and management
- Grocery list creation (manual and AI-generated)
- AI-powered recipe suggestions based on pantry items
- Recipe discovery from external API (TheMealDB)
- User settings for AI API configuration
- Responsive design for mobile and desktop

## Related Documentation
- [Components](/openwiki/frontend-client/components.md) - Detailed documentation of reusable UI components
<!-- openwiki: broken internal link [/openwiki/frontend-client/contexts.md] file "/openwiki/frontend-client/contexts.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Contexts](/openwiki/frontend-client/contexts.md) - State management providers and hooks
- [Pages](/openwiki/frontend-client/pages.md) - Route-specific page components
- [Services](/openwiki/frontend-client/services.md) - API service layer and endpoints
<!-- openwiki: broken internal link [/openwiki/frontend-client/entry-points-and-styles.md] file "/openwiki/frontend-client/entry-points-and-styles.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Entry Points and Styles](/openwiki/frontend-client/entry-points-and-styles.md) - Application entry points and styling approach
- [Backend API](/openwiki/backend-api/) - Documentation of the API endpoints consumed by the frontend
---
type: Component Documentation
title: Frontend Client Contexts
description: Documentation of React Context API usage for global state management in the FridgePilot frontend client.
tags: [frontend, client, react, context, state-management]
---
# Frontend Client Contexts

## Overview

The frontend uses React Context API for global state management. This approach avoids prop drilling and provides a centralized way to manage state that is shared across multiple components.

## Contexts

### AuthContext
Manages user authentication state including:
- Login status (logged in/out)
- User data (email, ID, etc.)
- JWT token handling
- Login, logout, and registration functions

### PantryContext
Manages pantry items state including:
- List of pantry items
- Loading states for fetch operations
- Functions to add, update, remove, and fetch pantry items
- Filtering and sorting capabilities

### RecipesContext
Manages recipes state including:
- User-created recipes
- Discovered recipes from external APIs (like TheMealDB)
- Loading states for recipe operations
- Functions to create, update, delete, and fetch recipes
- Search and matching functionality

### GroceryListsContext
Manages grocery lists state including:
- List of grocery lists
- Items within each list
- Loading states for list operations
- Functions to create, update, delete lists and list items
- Check-off functionality for items

### AIContext
Manages AI feature state including:
- Recipe suggestions based on pantry contents
- Generated grocery lists from recipes
- Loading states for AI requests
- Configuration for AI provider (NVIDIA NIM)
- Error handling for AI service unavailability

## Implementation

Each context follows a similar pattern:
1. Create a context using `React.createContext()`
2. Create a provider component that wraps the application or sub-tree
3. Create a custom hook (e.g., `useAuth`) to consume the context
4. Export the provider and hook for use in components

## Related Documentation

<!-- openwiki: broken internal link [../overview.md] file "../overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Frontend Client Overview](../overview.md) - Frontend architecture
- [Components](./components.md) - Reusable UI components
- [Pages](./pages.md) - Page-level components
- [Services](./services.md) - API service layer
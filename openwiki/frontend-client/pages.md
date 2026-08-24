---
type: overview
title: Frontend Pages
description: Overview of the pages in the FridgePilot frontend application.
tags: ['frontend', 'pages', 'routing']
---
# Frontend Pages

The FridgePilot frontend is a single-page application (SPA) built with React Router. Each page corresponds to a route and is located in the `src/pages/` directory.

## Page List

- **Login** (`src/pages/Login.tsx`): Authentication page for existing users.
- **Signup** (`src/pages/Signup.tsx`): Registration page for new users.
- **Pantry** (`src/pages/Pantry.tsx`): View and manage pantry items.
- **Recipes** (`src/pages/Recipes.tsx`): View, create, and manage user recipes.
- **Grocery List** (`src/pages/GroceryList.tsx`): View and manage grocery lists and items.
- **Settings** (`src/pages/Settings.tsx`): Configure user settings, including AI provider configuration.
- **AI Suggestions** (`src/pages/AISuggestions.tsx`): View AI-generated recipe suggestions based on pantry items.
- **Discover** (`src/pages/Discover.tsx`): Discover recipes from TheMealDB based on available ingredients.

## Routing

Pages are rendered based on the URL path defined in `src/App.tsx` using React Router. Protected pages (those requiring authentication) are wrapped with the `RequireAuth` component.

## Common Layout

Most pages share a common layout that includes:
- A header with navigation and user profile.
- A main content area specific to the page.
- Optional sidebars or modals for additional functionality.

## Styling

Pages use global styles from `src/index.css` and may include component-specific styles. Some pages reference styles from `src/assets/` or inline styles.

## Related Files

- `src/App.tsx`: Defines the routes and renders the appropriate page component.
- `src/components/`: Reusable UI components used across pages.
- `src/services/`: API service layer used by pages to communicate with the backend.
- `src/contexts/`: React contexts for state management (if implemented).

## Testing

Each page can be tested using React Testing Library or similar frameworks to ensure rendering, user interactions, and data fetching work as expected.
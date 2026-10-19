---
type: Component
title: Frontend Entry Points and Styles
description: Entry points, styling conventions, and CSS architecture for the FridgePilot frontend client.
tags: [frontend, styling, css, entry-points]
---
# Frontend Client: Entry Points and Styles

## Overview

This document describes the entry points, styling conventions, and CSS architecture used in the FridgePilot frontend client.

## Entry Points

The frontend application has the following entry points:

### Main Entry Point
- `src/main.tsx` - Root React application entry point
- Initializes React 18 with strict mode
- Sets up routing, providers, and renders the App component

### Vite Configuration
- `vite.config.ts` - Build tool configuration
- Defines build optimizations, plugins, and server settings
- Configured for React 19 with TypeScript support

### Environment Variables
- `.env` files - Environment-specific configuration
- `VITE_API_URL` - Backend API endpoint
- `VITE_APP_NAME` - Application display name

## Styling Approach

### CSS Architecture
- **BEM-like conventions**: Block__Element--Modifier naming
- **Scoped styles**: Component-level CSS modules or CSS-in-JS
- **Utility classes**: Spacing, typography, and layout utilities
- **Dark mode support**: CSS variables for theme colors

### Style Organization
- `src/styles/` - Global styles and CSS variables
- `src/components/*/styles.module.css` - Component-specific styles
- `src/assets/` - Images, icons, and other static assets

### CSS Variables
Defined in `:root` for theme customization:
```css
:root {
  --color-primary: #2563eb;
  --color-secondary: #64748b;
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-error: #ef4444;
  --color-background: #f8fafc;
  --color-surface: #ffffff;
  --color-text-primary: #1e293b;
  --color-text-secondary: #64748b;
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;
}
```

## Component Styling Guidelines

### Button Variants
- **Primary**: `--color-primary` background, white text
- **Secondary**: `--color-surface` background, `--color-primary` text, border
- **Danger**: `--color-error` background, white text
- **Ghost**: Transparent background, `--color-primary` text on hover

### Form Elements
- Inputs: `--color-surface` background, `--color-text-primary` text, `--color-secondary` border
- Focus state: `--color-primary` ring, `--color-surface` background
- Error state: `--color-error` border, `--color-error` text

### Cards and Containers
- Background: `--color-surface`
- Border: `--color-secondary` (subtle)
- Shadow: 0 1px 3px rgba(0,0,0,0.1)
- Radius: `--radius-md`

## Responsive Design

### Breakpoints
- Mobile: < 640px
- Tablet: 640px - 1024px
- Desktop: > 1024px

### Approach
- Mobile-first styling
- Flexbox and Grid for layouts
- Relative units (rem, em) for typography
- Viewport units for hero sections

## Related Documentation

<!-- openwiki: broken internal link [../overview.md] file "../overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Frontend Client Overview](../overview.md) - Frontend architecture
- [Components](./components.md) - Reusable UI components
- [Contexts](./contexts.md) - Global state management
- [Pages](./pages.md) - Page-level components
- [Services](./services.md) - API service layer
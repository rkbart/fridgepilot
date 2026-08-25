# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack

React 19 + TypeScript + Vite (frontend), Ruby on Rails 8 + PostgreSQL (backend), deployed on Google Cloud Run (API) and Vercel (client)

## Users

Home cooks managing groceries and meal-planning families who want to track what's in their pantry, discover recipes from available ingredients, and build smart grocery lists — reducing food waste and simplifying meal planning.

## Product Purpose

FridgePilot is a smart kitchen companion that helps users track pantry inventory, discover recipes they can make with what they already have, and generate grocery lists for missing ingredients. It reduces food waste, simplifies meal planning, and eliminates the guesswork of "what's for dinner?" by starting with what's already on hand.

## Positioning

All-in-one kitchen companion that starts with your pantry, not a recipe book. Unlike recipe-focused apps (Paprika, Mealime) or list-focused apps (AnyList), FridgePilot begins with what you already have and builds outward — suggesting recipes from your inventory, then generating shopping lists only for what's missing.

## Operating Context

- **Home kitchen environment**: Users check pantry before shopping, plan meals weekly, cook 3-5 times per week
- **Grocery shopping workflow**: Discover recipes → check pantry → build grocery list → shop → cook
- **Multi-user households**: Families or roommates sharing pantry visibility and grocery lists
- **Optional AI assistance**: Users can leverage AI for recipe suggestions or manage everything manually

## Capabilities and Constraints

### Core Features
- User accounts with email/password authentication (JWT)
- Pantry tracking with name, quantity, unit, category, and expiry dates
- Recipe management with ingredients, instructions, and photos
- Recipe discovery matching pantry against 300+ recipes from TheMealDB
- Grocery list CRUD with quantity, unit dropdown, and status check-off
- One-click add missing ingredients from recipes to grocery lists
- Optional AI assistant for recipe suggestions and grocery list generation

### Technical Constraints
- SPA architecture with separate frontend (Vite/React) and backend (Rails API)
- PostgreSQL database with case-insensitive duplicate prevention
- JWT authentication with JTI revocation strategy
- API deployed on Google Cloud Run (scale-to-zero)
- Client deployed on Vercel with SPA rewrites

### Decided Product Facts
- Pantry items prevent duplicates case-insensitively ("Spaghetti" and "spaghetti" can't coexist)
- Recipe discovery shows match percentages (Perfect Match / Almost There / More Needed)
- Grocery items have status (pending/confirmed/checked) and source (manual/ai_suggested)
- Optional AI integration via NVIDIA NIM provider

## Brand Commitments

No existing brand assets, logos, color palette, or typography guidelines. Fresh design opportunity.

## Evidence on Hand

- Working application with full CRUD for pantry, recipes, and grocery lists
- Recipe discovery integration with TheMealDB API (300+ recipes)
- Optional AI integration with NVIDIA NIM for recipe suggestions
- Docker Compose setup for local development
- Cloud Build configuration for API deployment

## Product Principles

1. **Pantry-first**: Start with what users have, not what they need to buy
2. **Reduce waste**: Help users use existing ingredients before they expire
3. **Simplify decisions**: Make "what's for dinner?" easy by suggesting recipes from available ingredients
4. **Flexible workflow**: Support both manual management and AI-assisted suggestions
5. **Family-friendly**: Enable shared visibility and collaboration in household meal planning

## Accessibility & Inclusion

No specific product-level accessibility requirements established yet. Standard web accessibility best practices should be followed.

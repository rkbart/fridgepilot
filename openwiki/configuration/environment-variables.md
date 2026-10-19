---
type: Configuration Guide
title: Environment Variables
description: Overview of environment variables used across the FridgePilot project.
tags: [configuration, environment-variables]
---
# Environment Variables

This document outlines the environment variables used in the FridgePilot project, covering both the API and client applications.

## API Environment Variables

The FridgePilot API (Ruby on Rails) uses the following environment variables:

- `DATABASE_URL`: Connection string for the PostgreSQL database
- `RAILS_ENV`: Rails environment (development, test, production)
- `RACK_ENV`: Rack environment (typically mirrors RAILS_ENV)
- `JWT_SECRET_KEY`: Secret key used for signing JWT tokens
- `NVIDIA_NIM_API_KEY`: API key for NVIDIA NIM service (optional, for AI features)
- `RAILS_LOG_TO_STDOUT`: Set to true to enable logging to stdout (important for Docker and Cloud Run)
- `RAILS_SERVE_STATIC_FILES`: Set to true to allow Rails to serve static files (though API is API-only)

## Client Environment Variables

The FridgePilot client (React/Vite) uses the following environment variables:

- `VITE_API_URL`: Base URL for the FridgePilot API (e.g., https://api.fridgepilot.example.com)
- `VITE_NVIDIA_NIM_API_KEY`: API key for NVIDIA NIM service (optional, for AI features) - exposed to client

Note: Client environment variables must be prefixed with `VITE_` to be exposed to the Vite-built application.

## Development Environment Variables

For local development, environment variables are typically set in a `.env` file at the root of each project (API and client repositories). Example:

```env
# API .env
DATABASE_URL=postgresql://user:password@localhost:5432/fridgepilot_development
RAILS_ENV=development
RACK_ENV=development
JWT_SECRET_KEY=dev-secret-key-change-in-production
NVIDIA_NIM_API_KEY=your-nvidia-nim-api-key
RAILS_LOG_TO_STDOUT=true
RAILS_SERVE_STATIC_FILES=true
```

```env
# Client .env
VITE_API_URL=http://localhost:3000
VITE_NVIDIA_NIM_API_KEY=your-nvidia-nim-api-key
```

## Production Environment Variables

In production (Google Cloud Run for API, Vercel for client), environment variables are set through the respective platform's interface:

- **API (Cloud Run)**: Set in the Cloud Run service configuration
- **Client (Vercel)**: Set in the Vercel project settings

## Related Documentation

- [API Service](../services/api.md) - Backend API details
- [Client Service](../services/client.md) - Frontend client details
- [Deployment - Cloud Run](../deployment/cloud-run.md) - API deployment specifics
- [Deployment - Vercel](../deployment/vercel.md) - Client deployment specifics
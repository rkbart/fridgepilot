---
type: Deployment Guide
title: Local Docker Compose Deployment
description: Instructions for running the full FridgePilot stack locally using Docker Compose.
tags: [deployment, docker, local-dev]
---
# Local Docker Compose Deployment

## Overview

This document describes how to run the complete FridgePilot stack (PostgreSQL database, API backend, and client frontend) locally using Docker Compose. This is useful for development, testing, and demonstration purposes.

## Prerequisites

- Docker and Docker Compose installed
- At least 4GB of RAM available (PostgreSQL, Rails, and Node.js containers)
- Ports 5432 (PostgreSQL), 3001 (API), and 5173 (client) available on localhost

## Configuration

The `docker-compose.yml` file defines three services:

1. **db** - PostgreSQL database
2. **backend** - FridgePilot API (Ruby on Rails 8)
3. **frontend** - FridgePilot client (React 19 TypeScript SPA)
4. **nginx** - Reverse proxy to route traffic to backend and frontend

## Usage

### Starting the Stack

```bash
docker compose up --build
```

This command:
1. Builds Docker images for the backend and frontend
2. Starts all services in the background
3. Sets up the network between services

### Accessing the Applications

Once the stack is running:
- **Client (SPA)**: <http://localhost:5173>
- **API**: <http://localhost:3001>
- **PostgreSQL**: localhost:5432 (for direct database access if needed)

### Stopping the Stack

```bash
docker compose down
```

This stops all containers and removes the network. To also remove named volumes (including PostgreSQL data):

```bash
docker compose down -v
```

### Viewing Logs

```bash
# View logs for all services
docker compose logs

# View logs for a specific service
docker compose logs backend
docker compose logs frontend
docker compose logs db
```

### Rebuilding Services

If you change the application code:

```bash
# Rebuild and restart specific service
docker compose up --build backend
docker compose up --build frontend

# Or rebuild everything
docker compose up --build
```

## Service Details

### Database (PostgreSQL)
- Image: `postgres:15-alpine`
- Ports: `5432:5432` (host:container)
- Environment: 
  - `POSTGRES_USER=fridgepilot`
  - `POSTGRES_PASSWORD=fridgepilot`
  - `POSTGRES_DB=fridgepilot_development`
- Volume: `postgres_data` persists database between restarts

### Backend (API)
- Build context: `./backend` (expects fridgepilot-api code in sibling directory)
- Ports: `3001:3001`
- Environment: 
  - `DATABASE_URL=postgresql://fridgepilot:fridgepilot@db:5432/fridgepilot_development`
  - `RAILS_ENV=development`
  - `RAILS_LOG_TO_STDOUT=true`
- Depends on: db

### Frontend (Client)
- Build context: `./frontend` (expects fridgepilot-client code in sibling directory)
- Ports: `5173:5173`
- Environment:
  - `VITE_API_URL=http://localhost:3001`
- Depends on: backend

### Nginx Reverse Proxy
- Image: `nginx:alpine`
- Ports: `80:80`
- Configuration: Routes `/api*` to backend and everything else to frontend
- Depends on: backend, frontend

## Development Workflow with Docker Compose

### Making Changes to Backend
1. Edit code in the `fridgepilot-api` repository (sibling directory)
2. Changes are immediately visible because the backend directory is mounted as a volume
3. For gem dependencies changes, rebuild: `docker compose up --build backend`

### Making Changes to Frontend
1. Edit code in the `fridgepilot-client` repository (sibling directory)
2. Changes are immediately visible due to Vite's hot module replacement and volume mounting
3. For npm dependencies changes, rebuild: `docker compose up --build frontend`

### Database Migrations
To run database migrations against the Dockerized database:

```bash
docker compose exec backend rails db:migrate
```

To reset the database (destroy all data):

```bash
docker compose exec backend rails db:reset
```

## Troubleshooting

### Port Conflicts
If you already have services running on ports 5432, 3001, or 5173:
1. Stop the conflicting services, or
2. Modify the port mappings in `docker-compose.yml`

### Container Health
Check container status:
```bash
docker compose ps
```

View detailed container information:
```bash
docker compose inspect backend
```

### Slow Startup
First startup may take several minutes as images are built and dependencies installed.
Subsequent startups are much faster due to Docker layer caching.

### Volume Permissions
If you encounter permission issues with mounted volumes:
1. Ensure your user has appropriate permissions to the sibling repositories
2. Consider using `:cached` or `:delegated` in volume mounts for better performance on macOS

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System architecture context
<!-- openwiki: broken internal link [./services/api.md] file "./services/api.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [API Service](./services/api.md) - Backend service details
<!-- openwiki: broken internal link [./services/client.md] file "./services/client.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Client Service](./services/client.md) - Frontend service details
<!-- openwiki: broken internal link [./development/local.md] file "./development/local.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Development Workflow](./development/local.md) - Alternative local development approaches
- [Cloud Run Deployment](./cloud-run.md) - Production deployment instructions
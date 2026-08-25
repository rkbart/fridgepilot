---
type: Development Guide
title: Local Development Workflow
description: Instructions for setting up and running FridgePilot locally for development.
tags: [development, local, setup]
---
# Local Development Workflow

## Overview

FridgePilot consists of two separate applications: a Ruby on Rails API and a React TypeScript client. Each has its own setup process and development workflow. This guide covers running both applications locally for development.

## Prerequisites

### For API (Ruby on Rails)
- Ruby (version specified in fridgepilot-api's Gemfile or .ruby-version)
- Bundler
- PostgreSQL
- Node.js (for JavaScript runtime in Rails)

### For Client (React)
- Node.js (version specified in fridgepilot-client's package.json or .nvmrc)
- npm or yarn

## API Setup

The API code lives in the separate fridgepilot-api repository. Follow these steps to set it up locally:

1. **Clone the repository**:
   ```bash
   git clone https://github.com/rkbart/fridgepilot-api.git backend
   cd backend
   ```

2. **Install dependencies**:
   ```bash
   bundle install
   ```

3. **Set up the database**:
   ```bash
   # Create and migrate the database
   rails db:create db:migrate
   
   # Optional: Load seed data if available
   # rails db:seed
   ```

4. **Set environment variables**:
   Create a `.env` file in the backend directory with:
   ```
   # JWT secret for token signing
   JWT_SECRET_KEY=your-secret-key-here
   
   # Database URL (if not using defaults)
   # DATABASE_URL=postgres://username:password@localhost:5432/fridgebot_development
   ```

5. **Start the server**:
   ```bash
   rails server -p 3001
   ```
   The API will be available at http://localhost:3001

## Client Setup

The client code lives in the separate fridgepilot-client repository. Follow these steps to set it up locally:

1. **Clone the repository**:
   ```bash
   git clone https://github.com/rkbart/fridgepilot-client.git frontend
   cd frontend
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Set environment variables**:
   Create a `.env` file in the frontend directory with:
   ```
   VITE_API_URL=http://localhost:3001
   ```

4. **Start the development server**:
   ```bash
   npm run dev
   ```
   The client will be available at http://localhost:5173

## Using Docker Compose (Alternative)

For a complete local stack with database, API, and client, you can use Docker Compose:

1. **Clone the required repositories**:
   ```bash
   git clone https://github.com/rkbart/fridgepilot-api.git backend
   git clone https://github.com/rkbart/fridgepilot-client.git frontend
   ```

2. **Start the stack**:
   ```bash
   docker compose up --build
   ```

3. **Access the applications**:
   - Client: http://localhost:5173
   - API: http://localhost:3001
   - PostgreSQL: localhost:5432

## Development Workflow

### API Development
- Run tests: `rails test` or `rspec` (depending on testing setup)
- Run linters: `rubocop` (if configured)
- View logs: Check terminal output or use `rails logger`
- Database changes: Generate migrations with `rails generate migration`

### Client Development
- Run tests: `npm test` (if configured)
- Run linters: `npm run lint` (if configured)
- Build for production: `npm run build`
- Hot module replacement: Automatic during `npm run dev`

## Common Tasks

### Database Operations
```bash
# Access PostgreSQL console
psql -h localhost -U fridgebot -d fridgebot_development

# Reset database (development only)
rails db:drop db:create db:migrate

# Check current schema
rails db:schema:dump
```

### Environment Variables
Both applications use environment variables for configuration. Key variables include:

**API (`backend/.env`)**:
- `JWT_SECRET_KEY`: Secret for signing JWT tokens
- `DATABASE_URL`: PostgreSQL connection string
- `RAILS_ENV`: Usually `development` for local work

**Client (`frontend/.env`)**:
- `VITE_API_URL`: URL of the API backend (Vite prefixes with VITE_)

## Troubleshooting

### API Issues
- **"Could not find a valid gem"**: Run `bundle install` again
- **Database connection errors**: Ensure PostgreSQL is running and `DATABASE_URL` is correct
- **Port already in use**: Change the port in `rails server -p [PORT]` or stop the conflicting service
- **Missing JWT secret**: Set `JWT_SECRET_KEY` in `.env`

### Client Issues
- **"Cannot find module"**: Run `npm install` again
- **Proxy errors**: Ensure `VITE_API_URL` points to the correct API URL
- **Port already in use**: Vite will usually try the next available port automatically
- **Blank screen**: Check browser console for errors and ensure API is running

### Docker Compose Issues
- **Service fails to start**: Check logs with `docker compose logs [service-name]`
- **Database connection refused**: Ensure the database service is healthy before dependent services start
- **File not found in container**: Verify volume mounts in `docker-compose.yml`
- **Permission errors**: Adjust volume permissions or run Docker with appropriate user rights

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System architecture context
<!-- openwiki: broken internal link [./services/api.md] file "./services/api.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [API Service](./services/api.md) - Backend service details
<!-- openwiki: broken internal link [./services/client.md] file "./services/client.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Client Service](./services/client.md) - Frontend service details
<!-- openwiki: broken internal link [./deployment/docker-compose.md] file "./deployment/docker-compose.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Deployment - Local Docker Compose](./deployment/docker-compose.md) - Docker Compose setup
- [Features](./features/) - Detailed feature documentation
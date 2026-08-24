# FridgePilot

FridgePilot is a smart fridge & kitchen companion that helps you track what's in your pantry, plan recipes from ingredients you already have, and build grocery lists — with an optional AI assistant that suggests recipes and generates shopping lists.

## Repositories

This repo is the **parent / orchestration** repository. It contains the shared infrastructure, deployment config, and documentation. The application code lives in two dedicated repositories:

| Repository | Stack | Description |
|---|---|---|
| [fridgepilot-api](https://github.com/rkbart/fridgepilot-api) | Ruby on Rails 8 · PostgreSQL · Devise + JWT | JSON API for auth, pantry, recipes, grocery lists, and AI features |
| [fridgepilot-client](https://github.com/rkbart/fridgepilot-client) | React 19 · TypeScript · Vite | Browser frontend (SPA) |

## Features

- **User accounts** — email/password signup & login with JWT authentication
- **Pantry** — track items you have on hand (name, quantity, unit, category, expiry); duplicate entries are prevented case-insensitively ("Spaghetti" and "spaghetti" can't coexist)
- **Recipes** — store and manage your own recipes with ingredients, instructions, and photos (file upload or URL)
- **Recipe Discovery** — match pantry ingredients against 300+ recipes from TheMealDB with match percentages (Perfect Match / Almost There / More Needed)
- **Grocery lists** — create lists and manage items with full CRUD (quantity, unit dropdown, status check-off), plus one-click add missing ingredients from recipes with pre-filled quantity & unit from recipe data
- **AI assistant** (optional) — suggest recipes from your pantry contents and generate grocery lists from recipes
- **Settings** — per-user AI provider configuration (NVIDIA NIM)

## Architecture

```
┌─────────────┐   HTTPS/JSON   ┌──────────────┐   SQL   ┌─────────────┐
│  Client (SPA)│ ─────────────▶ │   API (Rails) │ ──────▶ │  PostgreSQL  │
│  Vite/React  │ ◀───────────── │  Puma/JWT    │ ◀────── │              │
└─────────────┘   JWT bearer    └──────────────┘         └─────────────┘
                                     │  HTTP
                                     ▼
                              ┌──────────────┐
                              │  AI provider │
                              │  (NVIDIA NIM)│
                              └──────────────┘
```

- **API** — Ruby on Rails 8 (API-only), Devise + `devise-jwt` (JTI revocation strategy), PostgreSQL.
- **Client** — React + TypeScript SPA built with Vite, React Router for navigation.
- **Deployment** — API on Google Cloud Run (see `cloudbuild.yaml`), client on Vercel, database on Neon or any PostgreSQL.

## Quickstart with Docker

The whole stack (Postgres + API + client behind nginx) runs with one command:

```bash
docker compose up --build
```

- Client: <http://localhost:5173>
- API: <http://localhost:3001>

> Note: the Docker Compose Postgres binds host port `5432`. If you already run PostgreSQL locally on `5432`, stop it first or change the port mapping.

## Local development

Each app has its own setup. See the README in the respective repo:

- [fridgepilot-api → Getting started](https://github.com/rkbart/fridgepilot-api#getting-started)
- [fridgepilot-client → Getting started](https://github.com/rkbart/fridgepilot-client#getting-started)

Typical flow:

```bash
# API (port 3001)
cd backend
bundle install
rails db:create db:migrate
rails server -p 3001

# Client (port 5173)
cd frontend
npm install
npm run dev
```

## Deployment

- **API** — `cloudbuild.yaml` builds the backend image and deploys to Cloud Run (region `us-central1`, scale-to-zero). Configure via Cloud Build triggers.
- **Client** — `vercel.json` in the client repo configures SPA rewrites and the build output.

## Repository layout

```
fridgepilot/
├── docker-compose.yml   # Local full-stack orchestration
├── cloudbuild.yaml      # Cloud Run deployment for the API
├── CLAUDE.md            # Agent behavioral guidelines (used by coding assistants)
└── README.md
```

## License

All rights reserved. No license is granted for redistribution or commercial use without prior written permission.
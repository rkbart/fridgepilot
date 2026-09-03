---
type: Reference
title: Repository Layout
description: Overview of the files and directories in the FridgePilot parent/orchestration repository.
tags: [repository, layout, structure]
---
# Repository Layout

## Overview

This repository serves as the parent/orchestration repository for the FridgePilot project. It does not contain the main application code for the API or client, but rather the shared infrastructure, deployment configuration, and documentation.

The application code lives in two separate repositories:
- [fridgepilot-api](https://github.com/rkbart/fridgepilot-api): Ruby on Rails 8 JSON API
- [fridgepilot-client](https://github.com/rkbart/fridgepilot-client): React 19 TypeScript SPA

## Files and Directories

| File/Directory | Purpose |
|----------------|---------|
| `docker-compose.yml` | Local full-stack orchestration (Postgres + API + client behind nginx) |
| `cloudbuild.yaml` | Cloud Build configuration for deploying the API to Google Cloud Run (located in the API repository) |
| `README.md` | This file - overview of the project, features, architecture, and setup instructions |
| `AGENTS.md` | Agent behavioral guidelines (used by coding assistants) |
| `CLAUDE.md` | Agent behavioral guidelines (used by coding assistants) |
| `.gitignore` | Git ignore rules |
| `/skills/` | Directory containing OpenWiki skills (used for documentation generation) |
| `/openwiki/` | Generated OpenWiki documentation (this wiki) |
| `/conversation_history/` | Directory for storing conversation histories (if applicable) |

## Detailed File Descriptions

### docker-compose.yml
Defines a local development stack that includes:
- PostgreSQL database
- FridgePilot API (built from the backend directory)
- FridgePilot client (built from the frontend directory)
- nginx reverse proxy to route traffic to the API and client

Used for local development and testing of the full stack.

### cloudbuild.yaml
Google Cloud Build configuration that:
1. Builds a Docker image of the API from the `./backend` directory
2. Pushes the image to Google Container Registry (or Artifact Registry)
3. Deploys the image to Google Cloud Run

Enables automated deployment of the API to a managed, scalable environment.

### README.md
The main documentation file that provides:
- Project overview and features
- Architecture diagram
- Repository structure
- Quickstart instructions (Docker Compose)
- Local development setup instructions for API and client
- Deployment information
- License information

### AGENTS.md and CLAUDE.md
Configuration files for AI coding assistants (like GitHub Copilot or Claude) that provide behavioral guidelines, project context, and instructions to improve the assistant's effectiveness when working on this codebase.

### .gitignore
Specifies which files and directories should be ignored by Git, such as:
- Dependency directories (node_modules, vendor)
- Build artifacts
- Environment files
- IDE-specific files
- OS-specific files

### /skills/
Contains the skills used by the OpenWiki system to generate and update this documentation. These are not part of the product but are used for documentation maintenance.

### /openwiki/
The generated OpenWiki documentation. This directory is created and updated by the OpenWiki system and contains all the wiki pages.

### /conversation_history/
Directory for storing conversation histories if the project uses AI-assisted development features that require logging interactions.

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System architecture context
- [Services](./services/) - Details on the API and client services
- [Deployment](./deployment/) - Deployment options and instructions
<!-- openwiki: broken internal link [./development/local.md] file "./development/local.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Development Workflow](./development/local.md) - Local development setup
- [Features](./features/) - Detailed feature documentation
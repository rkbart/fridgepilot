---
type: Documentation Homepage
title: FridgePilot Orchestration Wiki
description: Entry point for the FridgePilot orchestration repository documentation. Provides links to architecture, deployment, and development workflows.
tags: [quickstart, navigation]
---
# FridgePilot Orchestration Wiki

This wiki documents the FridgePilot orchestration repository, which coordinates the frontend and backend applications, deployment configurations, and shared infrastructure.

## Overview

FridgePilot is a smart fridge & kitchen companion consisting of two main applications:
- **API** ([fridgepilot-api](https://github.com/rkbart/fridgepilot-api)): Ruby on Rails 8 JSON API
- **Client** ([fridgepilot-client](https://github.com/rkbart/fridgepilot-client)): React 19 TypeScript SPA

This repository contains:
- Docker Compose for local full-stack orchestration
- Cloud Build configuration for API deployment to Google Cloud Run
- Shared documentation and agent guidelines

## Navigation

- [Architecture Overview](architecture/overview.md) - System architecture and component relationships
<!-- openwiki: broken internal link [deployment.md] file "deployment.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Deployment](deployment.md) - Local and production deployment instructions
<!-- openwiki: broken internal link [development.md] file "development.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Development Workflows](development.md) - Local setup and development procedures

## Quick Links

- API Repository: https://github.com/rkbart/fridgepilot-api
- Client Repository: https://github.com/rkbart/fridgepilot-client
- Docker Compose: `docker-compose.yml`
- Cloud Build: `cloudbuild.yaml`
---
type: Documentation Hub
title: FridgePilot Documentation
description: Entry point for the FridgePilot repository wiki. Provides high-level overview and navigation to all major components.
tags: [quickstart, navigation]
---
# FridgePilot Documentation

Welcome to the FridgePilot documentation. This wiki covers the parent/orchestration repository that ties together the FridgePilot API and client applications.

## Overview

FridgePilot is a smart fridge & kitchen companion that helps you track what's in your pantry, plan recipes from ingredients you already have, and build grocery lists — with an optional AI assistant that suggests recipes and generates shopping lists.

This repository contains the shared infrastructure, deployment configuration, and documentation. The application code lives in two dedicated repositories:
- [fridgepilot-api](https://github.com/rkbart/fridgepilot-api): Ruby on Rails 8 JSON API
- [fridgepilot-client](https://github.com/rkbart/fridgepilot-client): React 19 TypeScript SPA

## Navigation

Select a topic below to explore detailed documentation:

- [Architecture Overview](./architecture/overview.md)
- [Repository Layout](./repository/layout.md)
- [Services](./services/)
  - [API Service](./services/api.md)
  - [Client Service](./services/client.md)
- [Deployment](./deployment/)
  - [Local Docker Compose](./deployment/docker-compose.md)
  - [Cloud Run Deployment](./deployment/cloud-run.md)
- [Development Workflow](./development/local.md)
- [Features](./features/)
  - [Pantry](./features/pantry.md)
  - [Recipes](./features/recipes.md)
  - [Grocery Lists](./features/grocery-lists.md)
  - [AI Assistant](./features/ai-assistant.md)

## Getting Started

For local development, see the [Development Workflow](./development/local.md) guide.

For deployment information, see the [Deployment](./deployment/) section.

## Backlog

- None currently tracked.
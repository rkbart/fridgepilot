---
type: Deployment Guide
title: Vercel Deployment for Client
description: Instructions for deploying the FridgePilot client to Vercel.
tags: [deployment, vercel, frontend, spa]
---
# Vercel Deployment for Client

## Overview

The FridgePilot client is a static single-page application (SPA) built with React and Vite, deployed to Vercel for global CDN distribution and automatic deployments from Git pushes.

## Deployment Process

The client is deployed to Vercel via the Vercel Git integration. When connected to a GitHub repository, Vercel automatically:

1. Detects changes to the `main` branch (or configured branch)
2. Installs dependencies using `npm ci` or `yarn install`
3. Builds the application using `vite build`
4. Deploys the built static assets to Vercel's edge network

## Project Settings

In the Vercel project settings, the following are configured:

- **Framework**: Vite (detected automatically)
- **Build Command**: `vite build`
- **Output Directory**: `dist` (default for Vite)
- **Environment Variables**: 
  - `VITE_API_URL`: URL of the deployed API (e.g., https://api.fridgepilot.example.com)
  - `VITE_NVIDIA_NIM_API_KEY`: API key for NVIDIA NIM service (optional, for AI features)

## Environment Variables

Vercel exposes environment variables prefixed with `VITE_` to the client build process. These must be set in the Vercel project settings:

- `VITE_API_URL`: Base URL for the FridgePilot API
- `VITE_NVIDIA_NIM_API_KEY`: API key for NVIDIA NIM service (optional)

Note: Do not expose `NVIDIA_NIM_API_KEY` without the `VITE_` prefix, as it will not be available to the client.

## Local Development vs Production

During local development, the client uses environment variables from a `.env.local` file (not committed to version control). In production, the variables are set in Vercel's interface.

## Related Documentation

- [Client Service](../services/client.md) - Frontend client details
- [API Service](../services/api.md) - Backend API details
- [Deployment - Cloud Run](./cloud-run.md) - API deployment specifics
- [Environment Variables](../configuration/environment-variables.md) - Environment variable reference
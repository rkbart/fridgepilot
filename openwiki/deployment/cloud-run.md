---
type: Deployment Guide
title: Cloud Run Deployment for API
description: Instructions for deploying the FridgePilot API to Google Cloud Run using Cloud Build.
tags: [deployment, cloud-run, gcp, api]
---
# Cloud Run Deployment for API

## Overview

The FridgePilot API is deployed to Google Cloud Run using Cloud Build. The `cloudbuild.yaml` file in this repository defines the build and deployment process.

This setup provides:
- Containerized deployment of the Ruby on Rails API
- Automatic builds on code pushes (when connected to a trigger)
- Scale-to-zero capability to minimize costs when idle
- Managed platform with automatic scaling

## Deployment Process

The `cloudbuild.yaml` defines the following steps:

### 1. Build the Docker Image
```yaml
- name: 'gcr.io/cloud-builders/docker'
  args: ['build', '-t', 'gcr.io/$PROJECT_ID/fridgepilot-backend:$COMMIT_SHA', './backend']
```
- Builds a Docker image from the `./backend` directory
- Tags the image with the project ID, service name, and commit SHA
- Assumes the API code is located in a `backend/` directory relative to the cloudbuild.yaml

### 2. Push the Image to Container Registry
```yaml
- name: 'gcr.io/cloud-builders/docker'
  args: ['push', 'gcr.io/$PROJECT_ID/fridgepilot-backend:$COMMIT_SHA']
```
- Pushes the built image to Google Container Registry (GCR)
- Uses the same tag as the build step

### 3. Deploy to Cloud Run
```yaml
- name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  entrypoint: gcloud
  args:
    - 'run'
    - 'deploy'
    - 'fridgepilot-backend'
    - '--image'
    - 'gcr.io/$PROJECT_ID/fridgepilot-backend:$COMMIT_SHA'
    - '--region'
    - 'us-central1'
    - '--platform'
    - 'managed'
    - '--allow-unauthenticated'
    - '--set-env-vars'
    - 'RAILS_ENV=production,RACK_ENV=production,RAILS_LOG_TO_STDOUT=true,RAILS_SERVE_STATIC_FILES=true'
    - '--memory'
    - '512Mi'
    - '--cpu'
    - '1'
    - '--min-instances'
    - '0'
    - '--max-instances'
    - '2'
```
- Deploys the image to Cloud Run service named `fridgepilot-backend`
- Deployed to the `us-central1` region on the managed platform
- Allows unauthenticated access (the API handles authentication via JWT)
- Sets environment variables for production Rails configuration
- Allocates 512MiB memory and 1 CPU
- Configured for scale-to-zero (0 min instances) with a maximum of 2 instances

## Environment Variables

The deployment sets the following environment variables:
- `RAILS_ENV=production`
- `RACK_ENV=production`
- `RAILS_LOG_TO_STDOUT=true` (enables logging to stdout for Cloud Run)
- `RAILS_SERVE_STATIC_FILES=true` (allows Rails to serve static files, though the API is API-only)

## Required Configuration

For the deployment to work, the following must be set up in Google Cloud:

1. **Cloud Build API** enabled
2. **Cloud Run API** enabled
3. **Container Registry API** enabled (or Artifact Registry API)
4. A Google Cloud project with billing enabled
5. The `backend/` directory containing the API source code with a proper Dockerfile

## Backend Repository Requirements

The `./backend` directory (expected to contain the fridgepilot-api code) must include:
- A `Dockerfile` that builds a runnable Rails API image
- The Rails application code
- Proper configuration for production (database connection, secrets, etc.)

Note: The actual fridgepilot-api repository is maintained separately at https://github.com/rkbart/fridgepilot-api.

## Database Connection

The API requires a PostgreSQL database. In production, this should be set up separately (e.g., on Neon, Cloud SQL, or another PostgreSQL provider) and the connection string provided via the `DATABASE_URL` environment variable.

The `cloudbuild.yaml` does not set `DATABASE_URL`; it must be configured in the Cloud Run service settings separately.

## Setting Up Cloud Build Triggers

To automate deployments:

1. Go to Cloud Build > Triggers in the Google Cloud Console
2. Create a trigger that:
   - Listens to pushes to a specific branch (e.g., main)
   - Runs the `cloudbuild.yaml` in the repository
3. Ensure the trigger has the necessary permissions to:
   - Build and push images to Container Registry/Artifact Registry
   - Deploy to Cloud Run

## Manual Deployment

To manually trigger a build and deployment (if you have the gcloud SDK and appropriate permissions):

```bash
gcloud builds submit --config cloudbuild.yaml
```

This will:
1. Build the Docker image from the current code
2. Push it to Container Registry
3. Deploy it to Cloud Run

## Accessing the Deployed API

After deployment, the API will be available at a URL like:
```
https://fridgepilot-backend-<hash>-uc.a.run.app
```

You can find the exact URL in the Cloud Run console or in the output of the `gcloud builds submit` command.

## Related Documentation

<!-- openwiki: broken internal link [./architecture/overview.md] file "./architecture/overview.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Architecture Overview](./architecture/overview.md) - System architecture context
<!-- openwiki: broken internal link [./services/api.md] file "./services/api.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [API Service](./services/api.md) - Backend service details
<!-- openwiki: broken internal link [./deployment/docker-compose.md] file "./deployment/docker-compose.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Deployment - Local Docker Compose](./deployment/docker-compose.md) - Local development setup
<!-- openwiki: broken internal link [./services/client.md] file "./services/client.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Client Service](./services/client.md) - Frontend service details (deployed to Vercel separately)
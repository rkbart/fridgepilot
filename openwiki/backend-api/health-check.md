---
type: feature
title: Health Check
description: The health check endpoint used to verify the application is running.
tags: ['backend', 'health', 'monitoring']
---
# Health Check

The FridgePilot API includes a health check endpoint that returns a 200 status when the application is running without exceptions, and 500 otherwise. This endpoint is used by load balancers and uptime monitors.

## Endpoint

- **Path**: `/up`
- **Method**: `GET`
- **Controller**: `rails/health#show` (provided by Rails)
- **Route**: `get "up" => "rails/health#show", as: :rails_health_check`

## Response

- **Success (200)**: Returns a plain text response indicating the application is healthy.
- **Failure (500)**: Returns a plain text response indicating the application encountered an error during boot.

## Usage

The endpoint is typically configured in load balancers (e.g., Google Cloud Load Balancer) or uptime monitoring services (e.g., UptimeRobot) to periodically check the availability of the API.

## Implementation

The health check is a standard Rails endpoint that checks if the application boots without exceptions. It does not perform deep checks (e.g., database connectivity) but ensures the Ruby on Rails framework is operational.

## Related Files

- `config/routes.rb`: Defines the route for the health check.
- No custom controller is used; it relies on Rails' built-in health controller.
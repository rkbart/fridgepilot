---
type: Configuration Guide
title: CORS Configuration
description: Configuration for Cross-Origin Resource Sharing (CORS) in the FridgePilot API.
tags: [configuration, cors, api]
---
# CORS Configuration

The FridgePilot API is configured to allow Cross-Origin Resource Sharing (CORS) for specific origins to enable the frontend client to communicate with the API from different domains.

## Configuration Location

The CORS configuration for the API is located in the separate API repository at:
https://github.com/rkbart/fridgepilot-api

Specifically, it is configured in `config/initializers/cors.rb` or similar.

## Typical Configuration

A typical CORS configuration for development and production might look like:

```ruby
# config/initializers/cors.rb
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins 'http://localhost:3000', # Frontend development URL
            'https://your-frontend-domain.vercel.app' # Production frontend URL
    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

## Related Documentation

- [API Service](../services/api.md) - Backend API details
- [Environment Variables](./environment-variables.md) - Environment variable configuration
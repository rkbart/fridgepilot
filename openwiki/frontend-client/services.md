---
type: feature
title: Frontend Services
description: The service layer for API communication in the FridgePilot frontend.
tags: ['frontend', 'services', 'api']
---
# Frontend Services

The FridgePilot frontend includes a service layer for making HTTP requests to the backend API. This layer is located in `src/services/api.ts` and provides a centralized way to interact with the API endpoints.

## API Service (`src/services/api.ts`)

The `api.ts` file defines a fetch-based API client that automatically attaches the JWT token (if available) to requests and handles common response parsing.

### Key Features

- **Base URL**: Configured to point to the backend API (e.g., `http://localhost:3000/api/v1` in development).
- **Automatic Token Injection**: Retrieves the JWT token from localStorage (or state) and adds it to the `Authorization` header as `Bearer <token>`.
- **JSON Handling**: Automatically parses JSON responses and stringifies request bodies.
- **Error Handling**: Throws errors for non-2xx responses, which can be caught by calling components.
- **Method Wrappers**: Provides wrapper functions for common HTTP methods (GET, POST, PUT, DELETE) with type safety.

### Example Usage

```ts
import { api } from '@/services/api';

// GET request
const response = await api.get('/me');
// response.data contains the parsed JSON

// POST request
await api.post('/recipes', { name: 'Pancakes', ingredients: [], instructions: [] });
```

### Functions

- `get(url: string, config?: AxiosRequestConfig)`: Performs a GET request.
- `post(url: string, data?: unknown, config?: AxiosRequestConfig)`: Performs a POST request.
- `put(url: string, data?: unknown, config?: AxiosRequestConfig)`: Performs a PUT request.
- `delete(url: string, config?: AxiosRequestConfig)`: Performs a DELETE request.

Note: The actual implementation may use fetch directly or a wrapper like axios. In this codebase, it uses fetch.

## Related Files

- `src/services/api.ts`: The service implementation.
- `src/pages/`: Pages that use the service to fetch and update data.
- `src/contexts/`: Contexts that may use the service to manage state (if implemented).

## Testing

The service layer can be tested by mocking fetch or using a service like MSW (Mock Service Worker) to simulate API responses.
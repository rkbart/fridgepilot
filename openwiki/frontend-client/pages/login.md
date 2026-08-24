---
type: feature
title: Login Page
description: The login page for the FridgePilot frontend application.
tags: ['frontend', 'page', 'authentication']
---
# Login Page

The Login page allows users to authenticate into the FridgePilot application. It is located at `src/pages/Login.tsx`.

## Route
- Path: `/login`
- Component: `Login`

## Functionality

The login page presents a form with the following fields:
- Email
- Password

Upon submission, the form sends a request to the backend authentication endpoint (`/users/sign_in` via Devise) to obtain a JWT token. The token is then stored in the application's state (via the Auth context) and used for subsequent API requests.

## Implementation Details

### Authentication Flow
1. User enters email and password.
2. Form submission triggers a login request to the backend.
3. On success, the backend returns a JWT token in the `Authorization` header (as per Devise-JWT configuration).
4. The frontend stores the token (typically in local storage or state) and sets the user as authenticated.
5. On failure, an error message is displayed.

### Related Components and Services
- Uses the `api` service (located in `src/services/api.ts`) to make the authentication request.
- May use the `RequireAuth` component to protect routes after login.
- Updates the Auth context to reflect the logged-in state.

### Validation
- Basic HTML5 validation for email and password fields.
- May include additional client-side validation (e.g., required fields, password length).

## Related Files
- `src/pages/Login.tsx`: The page component.
- `src/services/api.ts`: Service for making API calls, including the login endpoint.
- `src/contexts/AuthContext.tsx`: Context for managing authentication state (if exists).
- `src/components/RequireAuth.tsx`: Component to protect routes (if used).

## Testing
- Unit tests for the Login component (if using a testing framework like Jest or Vitest).
- Integration tests simulating the login flow.
---
type: feature
title: Current User
description: GET /api/v1/me endpoint for retrieving the authenticated user's data.
---

# Current User

The **Current User** endpoint provides the authenticated user's data after verifying the JWT token.

## Endpoint
```http
GET /api/v1/me
```
**Headers**:
```
Authorization: Bearer <jwt_token>
```
**Response**:
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "User Name",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z"
  // Note: settings may be included if configured in the serializer
}
```
**Success Status**: 200 OK
**Error Status**: 
- 401 Unauthorized if token is missing, invalid, or expired
- 401 Unauthorized if token has been revoked (JTI strategy)

## Implementation

### Controller
- **File**: `/backend/app/controllers/api/v1/me_controller.rb`
- **Base Class**: `ApplicationController` (which includes authentication concerns)
- **Action**: `show`

### Authentication
- Uses `Devise::JWT::TestHelpers` in tests, but in production relies on the `devise-jwt` Warden strategy
- The `authenticate_user!` method (provided by Devise) is used as a before_action to validate the JWT token
- On success, `current_user` is set to the authenticated User record

### Serialization
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#user_serializer] heading anchor "user_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- By default, uses `UserSerializer` (see [Serializers](/openwiki/backend-api/serializers.md#user_serializer))
- Can be overridden to include or exclude specific attributes

## Usage in Frontend
The frontend typically calls this endpoint on:
- Application startup to verify the token and load user data
- After sign-in to populate user state
- Periodically to refresh user data (if needed)

Example (pseudo-code):
```javascript
const response = await fetch('/api/v1/me', {
  headers: {
    'Authorization': `Bearer ${localStorage.getItem('token')}`
  }
});
const user = await response.json();
```

## Testing
Focused tests are in `/backend/spec/requests/me_controller_spec.rb`:
- Verify that a valid token returns the user data
- Verify that an invalid or missing token returns 401
- Verify that a revoked token (after sign-out) returns 401

## Related Components
- [Authentication](/openwiki/backend-api/authentication.md) - How tokens are obtained and renewed
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#user_serializer] heading anchor "user_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- [Serializers](/openwiki/backend-api/serializers.md#user_serializer) - How the user object is serialized for the response
<!-- openwiki: broken internal link [/openwiki/configuration/index.md#devise-configuration] file "/openwiki/configuration/index.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Configuration](/openwiki/configuration/index.md#devise-configuration) - Devise and JWT setup
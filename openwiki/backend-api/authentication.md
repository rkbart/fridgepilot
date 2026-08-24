---
type: feature
title: Authentication
description: Devise + JWT authentication flow with token renewal and revocation (JTI strategy).
---

# Authentication

FridgePilot uses **Devise** for user authentication combined with **devise-jwt** for JSON Web Token (JWT) management using the JTI (JWT ID) revocation strategy.

## Overview

- **Token Format**: JWT bearer tokens
- **Expiration**: 1 hour
- **Renewal**: Sliding window renewal with `/users/token/renew` endpoint
- **Revocation**: Server-side revocation on sign-out using JTI strategy
- **Storage**: Frontend stores token in `localStorage`
- **Header**: `Authorization: Bearer <token>`

## Endpoints

### Sign Up
```http
POST /users
```
**Request Body**:
```json
{
  "user": {
    "email": "user@example.com",
    "password": "securepassword",
    "password_confirmation": "securepassword",
    "name": "Optional Name"
  }
}
```
**Response**: User object (no token returned - user must sign in to get token)

### Sign In
```http
POST /users/sign_in
```
**Request Body**:
```json
{
  "user": {
    "email": "user@example.com",
    "password": "securepassword"
  }
}
```
**Response**:
```json
{
  "user": {
    "id": 1,
    "email": "user@example.com",
    "name": "User Name",
    // ... other user fields
  }
}
```
**Headers**:
```
Authorization: Bearer <jwt_token>
```

### Sign Out
```http
DELETE /users/sign_out
```
**Headers**:
```
Authorization: Bearer <current_token>
```
**Effect**: Revokes the current token server-side (JTI strategy), making it unusable even if not expired.

### Token Renewal
```http
POST /users/token/renew
```
**Headers**:
```
Authorization: Bearer <valid-but-soon-to-expire_token>
```
**Response**:
```json
{
  "user": {
    // current user object
  }
}
```
**Headers**:
```
Authorization: Bearer <fresh_jwt_token_with_1_hour_expiration>
```
**Note**: Only works with tokens that are still valid (not expired). Once expired, user must sign in again.

## Implementation Details

### Controllers
- `Users::RegistrationsController` - Inherits from Devise for sign up
- `Users::SessionsController` - Inherits from Devise for sign in
- `Users::TokensController` - Custom controller for token renewal
- Devise handles sign out automatically

### JWT Strategy
- Uses JTI (JWT ID) revocation strategy
- Each token gets a unique identifier stored in the `jti` column of the `users` table
- On sign-out, the JTI is recorded as revoked
- Validation checks if token's JTI is in the revoked list

### Security
- Tokens expire after 1 hour to limit exposure
- Renewal prevents frequent re-login during active use
- Server-side revocation protects against token theft
- HTTPS required in production for secure token transmission

## Testing

Focused tests are in `/backend/spec/requests/`:
- Authentication request specs cover sign up, sign in, sign out, and token renewal flows
- Tests verify proper token headers in responses
- Tests confirm token revocation on sign out
- Tests validate renewal only works with non-expired tokens

## Related Components

- [Current User](/openwiki/backend-api/current-user.md) - Retrieve authenticated user data
<!-- openwiki: broken internal link [/openwiki/configuration/index.md#devise-configuration] file "/openwiki/configuration/index.md" does not exist. Fix the href or restore the target, then delete this comment. -->
- [Configuration](/openwiki/configuration/index.md#devise-configuration) - Devise and JWT initializer settings
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#user_serializer] heading anchor "user_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- [Serializers](/openwiki/backend-api/serializers.md#user_serializer) - User serialization for API responses
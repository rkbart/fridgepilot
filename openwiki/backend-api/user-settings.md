---
type: feature
title: User Settings
description: Endpoints for managing user preferences including AI API configuration.
---

# User Settings

The User Settings feature allows users to configure their preferences, particularly for AI integration settings like API keys and endpoints.

## Model
### User
- **File**: `/backend/app/models/user.rb`
- **Attributes for Settings**:
  - `ai_api_key`: Encrypted string - Stores the user's NVIDIA NIM API key (encrypted using Rails' `has_secure_token` or similar encryption)
  - `ai_api_endpoint`: String - Custom endpoint for AI service (defaults to NVIDIA NIM if not set)
- **Associations**:
  - has_many :recipes
  - has_many :pantry_items
  - has_many :grocery_lists
- **Devise Configuration**:
  - Uses JWT authentication with JTI revocation strategy
  - Standard modules: database_authenticatable, registerable, recoverable, rememberable, validatable

## API Endpoints
All endpoints are under `/api/v1/settings` and require authentication.

### GET /api/v1/settings
Retrieve current user's settings (with sensitive data masked).

**Response**:
```json
{
  "data": {
    "ai_api_key": "••••••••", // Masked if present, null if not set
    "ai_api_endpoint": "https://integrate.api.nvidia.com/v1", // Current endpoint or null
    "has_api_key": true // Boolean indicating if API key is set
  }
}
```
**Status**: 200 OK

### PATCH/PUT /api/v1/settings
Update user's settings.

**Request Body**:
```json
{
  "settings": {
    "ai_api_key": "your-nim-api-key-here",
    "ai_api_endpoint": "https://custom.ai.endpoint/v1"
  }
}
```
**Note**: Both fields are optional; only provided fields will be updated.

**Response**:
```json
{
  "data": {
    "message": "Settings updated."
  }
}
```
**Status**: 200 OK
**Error**: 422 Unprocessable Entity if validation fails (e.g., invalid format)

## Controller
- **File**: `/backend/app/controllers/api/v1/settings_controller.rb`
- **Base Class**: `Api::V1::BaseController`
- **Actions**: show, update
- **Authentication**: Uses `authenticate_user!` before_action (inherited)
- **Security**:
  - API key is stored encrypted in the database
  - When returned via API, it's masked as "••••••••" for security
  - Only the presence/absence is indicated via `has_api_key` boolean
  - Update action uses strong parameters (`settings_params`) to prevent mass assignment

## Testing
Focused tests are in:
- `/backend/spec/requests/settings_controller_spec.rb`

Tests verify:
- Authentication is required for both endpoints
- Settings can be retrieved with proper masking
- Settings can be updated successfully
- Invalid updates return appropriate error messages
- Encryption/decryption of API key works correctly
- Masking of sensitive data in responses

## Usage in Frontend
The frontend interacts with this API through the settings service:
- Loads user settings on profile/settings page load
- Masks API key in UI inputs (shows as hidden or obscured)
- Allows user to enter/save their NVIDIA NIM API key
- Allows user to configure custom AI endpoint (advanced use case)
- Shows success/error messages after update attempts
- Uses stored API key when calling AI integration endpoints
- Handles cases where API key is not yet configured

## Related Components
- [Authentication](/openwiki/backend-api/authentication.md) - Required for accessing settings
- [Current User](/openwiki/backend-api/current-user.md) - Settings belong to the current user
- [AI Integration](/openwiki/backend-api/ai-integration.md) - Uses the API key and endpoint from settings
<!-- openwiki: broken internal link [/openwiki/backend-api/serializers.md#user_serializer] heading anchor "user_serializer" does not exist in "/openwiki/backend-api/serializers.md". Fix the href or restore the target, then delete this comment. -->
- [Serializers](/openwiki/backend-api/serializers.md#user_serializer) - How user data is formatted (though settings endpoint builds custom JSON)
<!-- openwiki: broken internal link [/openwiki/backend-api/routing.md#apiv1basecontroller] heading anchor "apiv1basecontroller" does not exist in "/openwiki/backend-api/routing.md". Fix the href or restore the target, then delete this comment. -->
- [Backend API Base Controller](/openwiki/backend-api/routing.md#apiv1basecontroller) - Provides common functionality like authentication
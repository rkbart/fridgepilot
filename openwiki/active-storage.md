---
type: feature
title: Active Storage
description: File attachment handling for recipe images in the FridgePilot backend.
tags: ['backend', 'storage', 'file-upload']
---
# Active Storage

The FridgePilot application uses Rails Active Storage to handle file attachments, specifically for attaching images to recipes. This allows users to upload photos of their recipes which are then stored and served efficiently.

## Configuration

Active Storage is configured in the following files:

- `config/storage.yml`: Defines the storage services (local, AWS S3, Google Cloud Storage, etc.)
- `/backend/config/initializers/active_storage.rb`: Initializer for Active Storage settings (if present)

In the current codebase, the default configuration uses the `:local` service for development and test environments. Production would typically configure a cloud storage service.

## Model Attachment

The Recipe model has one attached image:

```ruby
# /backend/app/models/recipe.rb
class User < ApplicationRecord
  has_one_attached :image
  # ... other code
end
```

This creates:
- A polymorphic association through the Active Storage tables
- Methods like `recipe.image.attach(io)`, `recipe.image.attached?`, `recipe.image.url`

## Database Schema

Active Storage creates three tables (see [Database Schema](/openwiki/database-schema.md) for details):
1. `active_storage_blobs` - Stores file metadata
2. `active_storage_attachments` - Joins blobs to records
3. `active_storage_variant_records` - Tracks image transformations

## Image Processing

The application can generate different sizes/variants of uploaded images using Active Storage variants. In the RecipeSerializer, the `image_url` method handles this:

```ruby
# /backend/app/serializers/recipe_serializer.rb
def image_url
  return @recipe.image_url if @recipe.image_url.present?
  return nil unless @recipe.image.attached?

  Rails.application.routes.url_helpers.url_for(@recipe.image)
end
```

For resized images, you could use:
```ruby
recipe.image.variant(resize_to_limit: [100, 100]).processed
```

## File Uploads

When uploading a recipe with an image:
1. The frontend sends a multipart/form-data request with the image file
2. The Rails controller accepts the file as part of the recipe parameters
3. Active Storage stores the file and creates a blob record
4. An attachment record joins the blob to the recipe
5. The image can be accessed via a URL or through variants

## Storage Services

The application can be configured to use different storage services:
- **Local**: Files stored on the local filesystem (used in development/test)
- **Amazon S3**: Cloud storage on AWS
- **Google Cloud Storage**: Cloud storage on GCP
- **Microsoft Azure**: Cloud storage on Azure
- **Mirror**: Mirror files to multiple services

Configuration in `config/storage.yml` specifies which service to use per environment.

## Related Files

- `/backend/app/models/recipe.rb`: Model with `has_one_attached :image`
- `/backend/app/serializers/recipe_serializer.rb`: Serializer that exposes image URL
- `/backend/controllers/api/v1/recipes_controller.rb`: Controller that handles image uploads
- `config/storage.yml`: Storage service configuration
- `/backend/db/migrate/`: Migration files that created Active Storage tables

## Testing

Active Storage attachments can be tested by:
- Using fixture file uploads in tests
- Mocking the storage service
- Testing that URLs are generated correctly
- Verifying that attachments are created and destroyed with records

## Security Considerations

- File type validation should be implemented to restrict uploads to safe image types
- File size limits should be configured to prevent DoS attacks
- When using cloud storage, ensure proper access controls are set on buckets
- Consider using content delivery networks (CDNs) for serving images in production
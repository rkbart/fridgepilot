---
type: reference
title: Database Schema
description: Overview of the FridgePilot database schema and table relationships.
tags: ['backend', 'database', 'schema']
---
# Database Schema

The FridgePilot application uses a PostgreSQL database with the following schema. This document provides an overview of the tables, their columns, relationships, and indexes.

## Tables

### users
Stores user account information.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | bigint | PRIMARY KEY | Unique identifier |
| email | string | NOT NULL, UNIQUE | User's email address |
| encrypted_password | string | NOT NULL | Devise encrypted password |
| reset_password_token | string | | Token for password reset |
| reset_password_sent_at | datetime | | When reset token was sent |
| remember_created_at | datetime | | When remember token was created |
| name | string | | User's display name |
| provider | string | | Authentication provider (email, etc.) |
| ai_api_key | string | | Encrypted API key for AI service |
| ai_api_endpoint | string | | Custom endpoint for AI service |
| created_at | datetime | NOT NULL | Record creation timestamp |
| updated_at | datetime | NOT NULL | Record update timestamp |

**Indexes:**
- `index_users_on_email` (unique)
- `index_users_on_reset_password_token` (unique)

**Associations:**
- Has many :recipes
- Has many :pantry_items
- Has many :grocery_lists

### recipes
Stores user-created recipes.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | bigint | PRIMARY KEY | Unique identifier |
| name | string | | Recipe name |
| image_url | string | | URL to recipe image |
| ingredients | jsonb | NOT NULL | Array of ingredient objects |
| instructions | jsonb | NOT NULL, default: [] | Array of instruction strings |
| user_id | bigint | NOT NULL, FK -> users.id | Owner of the recipe |
| source | string | | Source of recipe (user, imported, etc.) |
| prep_time | integer | | Preparation time in minutes |
| cook_time | integer | | Cooking time in minutes |
| servings | integer | | Number of servings |
| cuisine | string | | Cuisine type |
| description | text | | Recipe description |
| created_at | datetime | NOT NULL | Record creation timestamp |
| updated_at | datetime | NOT NULL | Record update timestamp |

**Indexes:**
- `index_recipes_on_user_id`

**Associations:**
- Belongs to :user
- Has many :grocery_items (through recipe_id)

### pantry_items
Tracks items in the user's pantry.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | bigint | PRIMARY KEY | Unique identifier |
| name | string | NOT NULL | Item name |
| quantity | decimal | | Quantity of item |
| unit | string | | Unit of measurement (count, grams, etc.) |
| category | string | | Food category (dairy, produce, etc.) |
| expires_at | date | | Expiration date |
| user_id | bigint | NOT NULL, FK -> users.id | Owner of the pantry item |
| created_at | datetime | NOT NULL | Record creation timestamp |
| updated_at | datetime | NOT NULL | Record update timestamp |

**Indexes:**
- `index_pantry_items_on_user_id`
- `index_pantry_items_on_user_normalized_name` (unique) - Prevents duplicate items for same user (case-insensitive, trimmed)

**Associations:**
- Belongs to :user

### grocery_lists
Collections of items needed for shopping.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | bigint | PRIMARY KEY | Unique identifier |
| name | string | NOT NULL | List name |
| source | string | NOT NULL, inclusion: [manual, ai_generated] | How the list was created |
| user_id | bigint | NOT NULL, FK -> users.id | Owner of the grocery list |
| created_at | datetime | NOT NULL | Record creation timestamp |
| updated_at | datetime | NOT NULL | Record update timestamp |

**Indexes:**
- `index_grocery_lists_on_user_id`

**Associations:**
- Belongs to :user
- Has many :grocery_items

### grocery_items
Individual items within a grocery list.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | bigint | PRIMARY KEY | Unique identifier |
| name | string | NOT NULL | Item name |
| quantity | decimal | | Quantity needed |
| unit | string | | Unit of measurement |
| status | string | NOT NULL, inclusion: [pending, confirmed, checked] | Current status of item |
| source | string | NOT NULL, inclusion: [manual, ai_suggested] | How the item was added |
| recipe_id | bigint | FK -> recipes.id | Associated recipe (if from recipe) |
| grocery_list_id | bigint | NOT NULL, FK -> grocery_lists.id | Parent grocery list |
| created_at | datetime | NOT NULL | Record creation timestamp |
| updated_at | datetime | NOT NULL | Record update timestamp |

**Indexes:**
- `index_grocery_items_on_grocery_list_id`
- `index_grocery_items_on_recipe_id`

**Associations:**
- Belongs to :grocery_list
- Belongs to :recipe (optional)

### Active Storage Tables
The application uses Active Storage for handling file attachments (recipe images).

#### active_storage_blobs
Stores metadata about uploaded files.

| Column | Type | Constraints |
|--------|------|-------------|
| id | bigint | PRIMARY KEY |
| key | string | NOT NULL, UNIQUE |
| filename | string | NOT NULL |
| content_type | string | |
| metadata | text | |
| service_name | string | NOT NULL |
| byte_size | bigint | NOT NULL |
| checksum | string | |
| created_at | datetime | NOT NULL |

#### active_storage_attachments
Joins blobs to records.

| Column | Type | Constraints |
|--------|------|-------------|
| id | bigint | PRIMARY KEY |
| record_id | bigint | NOT NULL |
| record_type | string | NOT NULL |
| name | string | NOT NULL |
| blob_id | bigint | NOT NULL, FK -> active_storage_blobs.id |
| created_at | datetime | NOT NULL |

**Indexes:**
- `index_active_storage_attachments_on_blob_id`
- `index_active_storage_attachments_uniqueness` (unique) - Prevents duplicate attachments

#### active_storage_variant_records
Tracks image transformations.

| Column | Type | Constraints |
|--------|------|-------------|
| id | bigint | PRIMARY KEY |
| blob_id | bigint | NOT NULL, FK -> active_storage_blobs.id |
| variation_digest | string | NOT NULL |
| created_at | datetime | NOT NULL |
| updated_at | datetime | NOT NULL |

**Indexes:**
- `index_active_storage_variant_records_on_blob_id`
- `index_active_storage_variant_records_uniqueness` (unique)

## Relationships Summary

- **User** has many: Recipes, PantryItems, GroceryLists
- **Recipe** belongs to: User; has many: GroceryItems
- **PantryItem** belongs to: User
- **GroceryList** belongs to: User; has many: GroceryItems
- **GroceryItem** belongs to: GroceryList; belongs to: Recipe (optional)

## Important Constraints & Validations

1. **Pantry Item Uniqueness**: Users cannot have duplicate pantry items (case-insensitive, whitespace-normalized)
2. **Source Fields**: 
   - GroceryList.source: manual or ai_generated
   - GroceryItem.source: manual or ai_suggested
   - GroceryItem.status: pending, confirmed, or checked
3. **JSONB Fields**: Recipes.ingredients and Recipes.instructions are stored as JSONB for efficient querying
4. **Foreign Keys**: All relationships have proper foreign key constraints with cascading deletes where appropriate

## Schema Generation

This schema is managed through ActiveRecord migrations. The current schema can be found in `/backend/db/schema.rb` and is generated from the migration files in `/backend/db/migrate/`.

## Related Documentation

- [Active Storage](/openwiki/active-storage.md): File attachment handling
- [Backend API](/openwiki/backend-api/index.md): How the API interacts with the database
- [Models](/openwiki/backend-api/#model-documentation): Individual model documentation (pantry-items, recipes, grocery-lists, etc.)
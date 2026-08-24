---
type: feature
title: Frontend Components
description: Reusable UI components used across the FridgePilot frontend application.
tags: ['frontend', 'components', 'react']
---
# Frontend Components

The FridgePilot frontend contains reusable UI components located in `src/components/`. These components are built with React and TypeScript, and are used to construct the various pages of the application.

## Component List

### ChevronActions.tsx
A reusable component that renders a set of actions (typically edit and delete) with chevron icons. It accepts callback functions for edit and delete actions.

**Props:**
- `onEdit`: Function to call when the edit action is triggered.
- `onDelete`: Function to call when the delete action is triggered.
- `className?`: Optional CSS class for styling.

**Usage:**
```tsx
<ChevronActions onEdit={handleEdit} onDelete={handleDelete} />
```

### EditModal.tsx
A modal dialog component used for editing entities such as pantry items, recipes, and grocery items. It includes form fields, validation, and submit/cancel buttons.

**Props:**
- `isOpen`: Boolean controlling the visibility of the modal.
- `onClose`: Function to call when the modal is closed.
- `title`: The title of the modal.
- `children`: The form content to display inside the modal.
- `onSubmit`: Function to call when the form is submitted.
- `submitLabel`: Label for the submit button (default: "Save").
- `loading`: Boolean indicating if the submit action is in progress.

**Usage:**
```tsx
<EditModal
  isOpen={isEditing}
  onClose={closeEditModal}
  title="Edit Pantry Item"
  onSubmit={handleSubmit}
>
  {/* Form fields go here */}
</EditModal>
```

### RequireAuth.tsx
A higher-order component (HOC) or wrapper component that ensures the user is authenticated before rendering its children. If the user is not authenticated, it redirects to the login page.

**Props:**
- `children`: The component tree to render if authenticated.

**Usage:**
```tsx
<RequireAuth>
  <Dashboard />
</RequireAuth>
```

## Styling

Components use a combination of global CSS (from `index.css`) and component-specific styles. Some components may reference styles defined in `src/styles/` or use inline styles.

## Related Files

- `src/components/`: Directory containing all component definitions.
- `src/styles/`: Directory containing CSS files for component styling and design tokens.
- `src/index.css`: Global CSS styles applied to the application.
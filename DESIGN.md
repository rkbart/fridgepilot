---
name: FridgePilot
description: Smart kitchen companion for pantry tracking, recipe discovery, and grocery management
colors:
  primary: "#5F7161"
  primary-light: "#6B8F71"
  success: "#6B8F71"
  warning: "#D4A843"
  terracotta: "#C4704B"
  terracotta-light: "#D4A373"
  honey: "#D4A843"
  linen: "#FAF8F5"
  linen-warm: "#F5F5F0"
  charcoal: "#2C2C2C"
  stone: "#6B6560"
  border: "#E8E4DF"
  border-light: "#F0ECE7"
  white: "#FFFFFF"
  danger: "#B83A2A"
  danger-bg: "#FDF2F0"
typography:
  display:
    fontFamily: "Lora, Georgia, serif"
    fontWeight: 500
    lineHeight: 1.2
  body:
    fontFamily: "DM Sans, system-ui, sans-serif"
    fontSize: "16px"
    fontWeight: 400
    lineHeight: 1.5
  mono:
    fontFamily: "JetBrains Mono, ui-monospace, monospace"
rounded:
  sm: "8px"
  md: "12px"
  lg: "16px"
spacing:
  sm: "0.5rem"
  md: "1rem"
  lg: "2rem"
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.white}"
    rounded: "{rounded.sm}"
    padding: "0.5rem 1rem"
  button-secondary:
    backgroundColor: "{colors.white}"
    textColor: "{colors.charcoal}"
    rounded: "{rounded.sm}"
    padding: "0.5rem 1rem"
  button-danger:
    backgroundColor: "{colors.white}"
    textColor: "{colors.danger}"
    rounded: "{rounded.sm}"
    padding: "0.5rem 1rem"
  card:
    backgroundColor: "{colors.white}"
    rounded: "{rounded.md}"
    padding: "1.5rem"
---

# Design System: FridgePilot

## Overview

**Creative North Star: "The Modern Farmhouse"**

FridgePilot's design is a modern farmhouse: clean lines, honest materials, and functional elegance. Sage green grounds the interface like painted cabinetry; warm terracotta and honey appear as earthenware accents; soft cream backgrounds feel like morning light on plaster walls. Nothing is ornamental for its own sake—every surface earns its place the way a well-made kitchen tool does.

Typography pairs Lora, a serif with quiet warmth and just enough personality, for headings, with DM Sans for clean, modern body readability. The combination feels crafted rather than decorated: approachable, durable, calm under daily use.

**Key Characteristics:**
- Muted sage green as the working primary—calm, natural, never loud
- Terracotta and honey accents used like pottery on open shelves: sparingly and with purpose
- Warm neutral surfaces (cream, linen-warm) instead of clinical grays
- Soft, low shadows that suggest depth without drama
- Frosted modal backdrops and gentle 16px corners on large surfaces

## Colors

An earthy, muted palette built around sage green and warm neutrals, with terracotta and honey reserved for accent moments.

### Primary
- **Sage Green** (#5F7161): Primary actions, active navigation, links, checkbox fills. A muted green that reads as painted wood rather than neon signage.
- **Sage Light** (#6B8F71): Hover states and secondary emphasis; doubles as the success color.

### Accent
- **Terracotta** (#C4704B): Warm accents, category tags, "to buy" indicators.
- **Terracotta Light** (#D4A373): Soft backgrounds and muted decorative touches.
- **Honey** (#D4A843): Golden highlights and warnings; pairs with `--warning-bg` (#FBF5E6).

### Neutral
- **Charcoal** (#2C2C2C): Headings and body text.
- **Stone** (#6B6560): Secondary text, labels, meta information.
- **Cream / Linen** (#FAF8F5): Page background.
- **Linen Warm** (#F5F5F0): Hover rows, elevated surfaces, skeletons.
- **White** (#FFFFFF): Cards, inputs, modals.
- **Border** (#E8E4DF) / **Border Light** (#F0ECE7): Structural separation and subtle dividers.

### State Colors
- **Success** (#6B8F71) on **Success Background** (#EFF5EF): Pantry availability ✓, checked-off items.
- **Warning** (#9A7B2A text) on **Warning Background** (#FBF5E6): Partial availability ⚠.
- **Danger** (#B83A2A) on **Danger Background** (#FDF2F0): Destructive actions, missing ingredients ✗.

### Named Rules
**The Sage Restraint Rule.** The primary sage covers no more than ~15% of any screen—active states, primary buttons, and wayfinding only. Its quietness is what makes it trustworthy.

## Typography

**Display Font:** Lora (with Georgia, serif fallback)
**Body Font:** DM Sans (with system-ui, sans-serif fallback)
**Mono Font:** JetBrains Mono (with ui-monospace, monospace fallback)

**Character:** Lora brings bookish warmth to headings—the voice of a handwritten recipe card—while DM Sans keeps lists, forms, and labels crisp and legible at small sizes.

### Hierarchy
- **Display** (500 weight, 1.4–1.75rem, line-height 1.2): Brand, page titles, dashboard stat values.
- **Headline** (500 weight, 1.25rem, line-height 1.2): Section headings, card titles.
- **Body** (400 weight, 16px, line-height 1.5): Content, descriptions, form fields.
- **Label** (500–600 weight, 0.7–0.85rem, uppercase, letter-spacing 1.5px): Nav group labels, sidebar brand tag, section counts.
- **Mono** (JetBrains Mono): Quantities and measurements—data wears its own uniform.

### Named Rules
**The Recipe Card Rule.** Numerical data (quantities, counts) renders in mono; prose never does.

## Layout

Fixed 240px sidebar with grouped navigation (Home · Discover · Manage · Account), user profile footer, and a main content column capped at 900px. Spacing follows an 8px base rhythm: 0.5rem inside rows, 1–1.5rem within cards, 2rem between sections. Dashboard stats use an auto-fit grid collapsing to two columns on mobile; the bottom tab bar takes over navigation below 768px.

## Elevation & Depth

Depth is quiet and tonal. Surfaces sit flat on cream ground; cards lift with a whisper shadow; modals float above a frosted, blurred backdrop.

### Shadow Vocabulary
- **Rest** (`0 1px 2px rgba(44,44,44,0.05)`): Cards at rest.
- **Raised** (`0 4px 12px rgba(44,44,44,0.07)`): Hovered cards, dropdowns.
- **Floating** (`0 12px 32px rgba(44,44,44,0.10)`): Modals, toasts.

### Named Rules
**The Flat Counter Rule.** Shadows are responses, not decorations—an element lifts when you act on it, never merely because it exists.

## Shapes

Two-step radius language: 8px on controls (buttons, inputs, tags), 12px on cards and containers, 16px on large floating surfaces (modals). Status pills use full-round ends (1rem). Borders are hairline (#E8E4DF) and reserved for structure and input edges; hover states shift border color rather than adding weight.

## Components

### Buttons
- **Shape:** Rounded (8px)
- **Primary:** Sage Green fill (#5F7161), white text; hovers lighten to #6B8F71.
- **Secondary:** White with charcoal text and hairline border; hover tints text and border sage.
- **Danger:** White with danger-red text; hover fills with light red wash and red border.
- **Motion:** 0.15s ease transitions throughout.

### Cards / Containers
- **Corner Style:** 12px radius
- **Background:** White on cream page ground
- **Shadow Strategy:** Rest shadow, lifting to Raised on hover
- **Internal Padding:** 1–1.5rem

### Inputs / Fields
- **Style:** White field, hairline border, 8px radius
- **Focus:** Sage border plus soft sage ring (`0 0 0 3px rgba(95,113,97,0.15)`)

### Navigation
- **Style:** Fixed sidebar with grouped links; uppercase group labels in Stone
- **Active:** Sage text, tinted sage background, 3px sage left border
- **Footer:** Avatar initial in sage circle, name/email, Sign out below a hairline divider

### Availability Indicators
- **✓ Available:** Success green pill (#EFF5EF / #4F7057)
- **⚠ Partial:** Warning amber pill (#FBF5E6 / #9A7B2A)
- **✗ Missing:** Danger red pill (rgba red wash / #B83A2A)

## Do's and Don'ts

### Do:
- **Do** keep sage for interaction and state—buttons, links, active nav, checkboxes.
- **Do** use terracotta or honey when content needs warmth (tags, "to buy" markers).
- **Do** render quantities in JetBrains Mono.
- **Do** blur modal backdrops (`backdrop-filter: blur(4px)`) over a 40% charcoal scrim.
- **Do** scale radius with surface size: 8 → 12 → 16px.

### Don't:
- **Don't** introduce saturated greens or blues outside the palette—the muted tones are the identity.
- **Don't** stack heavy shadows; depth comes from tone first, shadow second.
- **Don't** use pure black (#000); text is Charcoal (#2C2C2C).
- **Don't** mix serif into body copy or labels; Lora lives in headings only.

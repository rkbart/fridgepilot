---
type: Design System
title: FridgePilot Design System
description: The design system for FridgePilot, including color palette, typography, and component styles.
tags: [design, frontend, ui]
---

# FridgePilot Design System

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

## Typography

### Display
- Font Family: "Lora, Georgia, serif"
- Font Weight: 500
- Line Height: 1.2

### Body
- Font Family: "DM Sans, system-ui, sans-serif"
- Font Size: "16px"
- Font Weight: 400
- Line Height: 1.5

### Mono
- Font Family: "JetBrains Mono, ui-monospace, monospace"

## Rounded
- sm: "8px"
- md: "12px"
- lg: "16px"

## Spacing
- sm: "0.5rem"
- md: "1rem"
- lg: "2rem"

## Component Examples

### Button Primary
- Background Color: `{colors.primary}`
- Text Color: `{colors.white}`
- Rounded: `{rounded.sm}`
- Padding: "0.5rem 1rem"

### Button Secondary
- Background Color: `{colors.white}`
- Text Color: `{colors.charcoal}`
- Rounded: `{rounded.sm}`
- Padding: "0.5rem 1rem"

### Button Danger
- Background Color: `{colors.white}`
- Text Color: `{colors.danger}`
- Rounded: `{rounded.sm}`
- Padding: "0.5rem 1rem"

### Card
- Background Color: `{colors.white}`
- Rounded: `{rounded.md}`
- Padding: "1.5rem"

---
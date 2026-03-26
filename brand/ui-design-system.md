<!-- Peblla Marketing Team Knowledge Base — Auto-generated from group chat learnings -->

# Peblla UI Design System

## Foundation

| Setting | Value |
|---------|-------|
| **Default Design System** | shadcn/ui (Neutral theme, OKLCH color tokens) |
| **Alternate Design System** | Material Design 3 (M3) — when Mina specifies |
| **Icon Set** | **Feather Icons only** (https://feathericons.com/) — no other icon libraries permitted |
| **Font** | **Roboto** (Google Fonts CDN) — keep original heading weights unchanged |
| **Border Radius** | 0.625rem |
| **Output Format** | Single-file HTML with inline CSS + complete color variables, browser-previewable |

### Product-Level Design System Mapping

| Product | Design System |
|---------|--------------|
| **Peblla Portal** | shadcn/ui |
| **MPOS & BiteMe** | Material Design 3 (M3) |
| **POS** | POS-specific design system (neither shadcn nor M3) |
| **Fallback** | shadcn/ui |

### CDN References

```html
<!-- Feather Icons -->
<script src="https://unpkg.com/feather-icons"></script>

<!-- Roboto Font -->
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
```

## Color Token System

### Primary

| Token | Value | Usage |
|-------|-------|-------|
| `--primary` | `#1A1A1A` | Important CTA buttons, active states |
| `--primary-container` | `#F5F5F5` | Container backgrounds |

### Secondary (Brand)

| Token | Value | Usage |
|-------|-------|-------|
| `--secondary` | `#FF4D00` | Brand color — use sparingly |
| `--secondary-container` | `rgba(255, 77, 0, 0.05)` | 5% transparency brand tint |

### Semantic

| Token | Value | Usage |
|-------|-------|-------|
| `--error` | `#FF3B30` | Error states |
| `--error-container` | `#F9DEDC` | Error background |
| `--positive` | `#34C759` | Success states |
| `--functional-blue` | `#007AFF` | Links, informational |

### Background & Surface

| Token | Value |
|-------|-------|
| `--background` | `#FFFFFF` |
| `--surface` | `#FFFFFF` |
| `--scrim` | `#000000` at 32% opacity |

### Surface Containers

| Level | Value |
|-------|-------|
| Lowest | `#E6E6E6` |
| Low | `#F2F2F2` |
| Normal | `#FFFFFF` |
| High | `#F5F5F5` |
| Highest | `#FAFAFA` |

### Text (On-Surface)

| Token | Value | Usage |
|-------|-------|-------|
| `--on-primary` | `#FFFFFF` | Text on primary backgrounds |
| `--on-primary-container` | `#1A1A1A` | Text on primary containers |
| `--on-secondary` | `#FFFFFF` | Text on brand backgrounds |
| `--on-secondary-container` | `#1A1A1A` | Text on brand containers |
| `--on-surface-high` | `#1A1A1A` | High-emphasis text |
| `--on-surface-medium` | `#666666` | Medium-emphasis text |
| `--disabled` | `#A3A3A3` | Disabled state text |

### Outline

| Token | Value |
|-------|-------|
| `--outline` | `#CCCCCC` |
| `--outline-variant` | `#E6E6E6` |
| `--outline-primary` | `#1A1A1A` |
| `--shadow` | `#000000` at 30% opacity |

### State Layers

| Type | 8% | 12% | 16% |
|------|-----|------|------|
| Darken | `rgba(0,0,0,0.08)` | `rgba(0,0,0,0.12)` | `rgba(0,0,0,0.16)` |
| Lighten | `rgba(255,255,255,0.08)` | `rgba(255,255,255,0.12)` | `rgba(255,255,255,0.16)` |

## Color Usage Strategy (Core Rules)

### Overall Principle

The UI is **black, white, and gray** dominant. Brand color is used with **extreme restraint**.

### Black (#1A1A1A) — Strictly Limited

Only use for:
1. Important clickable **CTA buttons** (e.g., Charge, Submit)
2. **Selected/active** Tag / Chip / Tab
3. **Pagination** active state
4. **FAB** (Floating Action Button)

### Gray (#666666) — All Informational Elements

Covers everything that is display-only (not interactive):
- Source Badge (POS / Online)
- Status Tag (Completed / Pending / Refunded)
- Icons
- Detail panel values
- Avatars
- Table row data

### Brand Orange (#FF4D00) — Minimal

- **Only** in Nav Rail selected state (light orange container)
- Right panel / detail area / table area: **all grayscale**, no brand color
- **One exception**: Checkout/payment primary CTA button may use `#FF4D00` (e.g., "Charge" button)

### Navigation Bar

- White background + gray text
- Never use black or brand-color backgrounds for navigation

### Delete / Remove Buttons

- Gray background (`#F5F5F5`) + disabled gray icon (`#A3A3A3`)
- Hover state: darken to deeper gray

## M3 Adaptation Rules

When using Material Design 3 components:
- Use M3 component language: Navigation Rail, Filter Chip, Side Sheet, FAB, Elevation
- **Replace all M3 default colors** with Peblla color tokens above
- Roboto font remains (same as default)

## UI Page Generation Flow

1. Determine Design System (default shadcn/ui, switch to M3 when specified)
2. Load Peblla complete color tokens (CSS variables) replacing defaults
3. Build layout: Navigation Rail / Side Sheet / Top App Bar / Filter Chips as appropriate
4. Fill with **real data** (not Lorem ipsum) — simulate Peblla restaurant POS scenarios
5. Strictly verify color usage: black only for CTA, gray for info, brand color minimal
6. Generate screenshot (1440x900, 2x resolution) for review
7. Iterate based on feedback, produce comparison screenshots each round

## Reference Implementations

These pages serve as quality benchmarks for all future UI work:

| Page | Description | Status |
|------|-------------|--------|
| `pos_transaction_m3.html` | POS Transaction Details (M3 + Peblla Token) | Approved after 8 rounds with Mina |
| `pos_ordering_m3.html` | POS Ordering — Lai Lai Malatang (M3 + Peblla Token) | Approved after 4 rounds with Mina |

## Order Tracker UI (v1.2.0)

- **Layout**: Ready / Preparing dual-column
- **Cards**: Order number + customer name grid
- **Delivery Icons**: DoorDash / Uber Eats / Grubhub
- **Left Panel**: Marketing advertisement area
- **Theme**: Pure black/white/gray, supports Light/Dark dual themes

## Product Scene Mockup Capability

- Composite UI screenshots onto laptop/tablet/phone mockups for product scene images
- Use Gemini image editing (gemini-edit.py) with UI screenshot + scene description prompt
- Scene types: office desk close shot, restaurant interior, trade show booth, etc.

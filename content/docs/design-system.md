# Design System

## Color Palette

| Token | Hex | Usage |
|-------|-----|-------|
| `Color.Crave.background` | `#F2ECDA` | App background (cream/ecru) |
| `Color.Crave.primaryText` | `#222222` | Headlines, buttons, primary text |
| `Color.Crave.secondaryText` | `#444444` | Captions, secondary info |
| `Color.Crave.accent` | `#B35A3B` | Terracotta highlights |
| `Color.Crave.cardBackground` | `#E2DED6` | Card surfaces |
| `Color.Crave.darkCard` | `#333333` | Dark-on-light cards, loading overlays |
| `Color.Crave.success` | `#4A7C59` | Success states, pantry indicators |
| `Color.Crave.error` | `red` | Error states |
| `Color.Crave.divider` | `gray/0.35` | Divider lines |
| `Color.Crave.subtleBorder` | `gray/0.18` | Card borders |

### Tag Category Colors

| Category | Hex | Usage |
|----------|-----|-------|
| Cuisine | `#6B8E7B` | Sage green tag chips |
| Diet | `#B35A3B` | Terracotta tag chips |
| Meal Type | `#5A7CA8` | Muted blue tag chips |
| Characteristic | `#9B7DB8` | Muted purple tag chips |

## Typography

Custom font system via `.craveFont()` modifier:

| Style | Size | Weight | Design | Use Case |
|-------|------|--------|--------|----------|
| `.largeTitle` | 32pt | Medium | Serif | App title ("Crave") |
| `.sectionHeader` | 20pt | Medium | Serif | Section dividers |
| `.body` | 16pt | Regular | Default | Body text |
| `.action` | 18pt | Medium | Default | Button labels (0.5 tracking) |
| `.caption` | 12pt | Medium | Default | Small text, metadata |

## Component Library

Defined in `DesignSystem/Components.swift`:

| Component | Description |
|-----------|-------------|
| `CravePrimaryButtonStyle` | Full-width pill button, dark fill |
| `CraveOutlineButtonStyle` | Full-width outline pill button |
| `CraveSecondaryButtonStyle` | Configurable color solid button |
| `CraveTextFieldStyle` | White background with subtle border |
| `CraveCardStyle` | White card with shadow and border |
| `CraveFAB` | Floating action button (bottom-right) |
| `CraveFABMenu` | FAB with dropdown menu |
| `CraveEmptyState` | Icon + title + message for empty views |
| `CraveLoadingOverlay` | Dark overlay with spinner |
| `CraveToast` | Top toast notification (success/error) |
| `FlowLayout` | Wrapping layout for tag/ingredient chips |

## Iconography

SF Symbols throughout:

| Feature | Icon |
|---------|------|
| Home | `house` |
| Recipes | `book.closed` |
| Pantry | `shippingbox` |
| Grocery | `basket` |
| Profile | `person` |
| Scan | `camera` |

## Dark Mode

Crave uses a **light-only palette**. `Color.Crave.*` values are static. The app forces `.environment(\.colorScheme, .light)` on the root TabView.

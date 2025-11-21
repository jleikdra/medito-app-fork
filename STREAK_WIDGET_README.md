# Streak Widget Documentation

This directory contains comprehensive documentation about the streak widget design in the Medito app.

## Documentation Files

### 📋 [STREAK_WIDGET_DESIGN.md](./STREAK_WIDGET_DESIGN.md)
**Complete Design Specification**

Start here for a complete overview of the streak widget design, including:
- Visual constants (sizes, padding, border radius)
- Color schemes for active/inactive states
- Layout structure and hierarchy
- User experience patterns
- File locations for all components

**Best for:** Understanding the overall design and getting oriented with the widget system.

---

### 🏗️ [STREAK_WIDGET_ARCHITECTURE.md](./STREAK_WIDGET_ARCHITECTURE.md)
**Technical Architecture & Data Flow**

Deep dive into the technical implementation:
- File structure tree with all related files
- Component interaction diagrams
- Data flow (in-app and widget updates)
- State management logic
- Provider relationships
- Testing infrastructure

**Best for:** Developers working on the codebase or making modifications to the streak widget.

---

### 🎨 [STREAK_WIDGET_DESIGN_REFERENCE.md](./STREAK_WIDGET_DESIGN_REFERENCE.md)
**Quick Reference for Designers**

Concise reference with all design tokens:
- Spacing & dimensions tables
- Color palettes (light/dark themes)
- Typography specifications
- Animation details
- Icon asset catalog
- State variants
- Edge cases

**Best for:** Designers implementing UI changes or creating design specifications.

---

## Quick Links

### File Locations

#### Flutter (In-App Widget)
- Main Widget: `/lib/views/home/widgets/stats/streak_circle.dart`
- Controller: `/lib/views/home/widgets/stats/streak_circle_controller.dart`
- Constants: `/lib/views/home/widgets/stats/streak_circle_constants.dart`

#### Android (Home Screen Widgets)
- Meditation Widget: `/android/app/src/main/kotlin/meditofoundation/medito/widget/MeditationWidget.kt`
- Consistency Widget: `/android/app/src/main/kotlin/meditofoundation/medito/widget/ConsistencyWidget.kt`
- Drawables: `/android/app/src/main/res/drawable/`

#### Service Layer
- Home Widget Service: `/lib/services/home_widget_service.dart`

---

## Key Design Values

| Property | Value | Context |
|----------|-------|---------|
| Border Radius | 30 dp | In-app widget |
| Primary Purple | #917DF0 | Brand color |
| Badge Color | #EF5E55 | New feature indicator |
| Animation Duration | 3 seconds | Gradient rotation |
| Icon Size | 20 dp (in-app), 24 dp (widget) | Fire icon |
| Calendar Display | 5 days | Activity history |

---

## Visual Overview

### In-App Widget States

**Active (Completed Today)**
- Animated purple gradient border
- Purple fire icon
- Bold text

**Inactive (Not Completed Today)**
- No border
- Gray fire icon
- Normal text weight

**First Time User**
- Coral/red badge indicator
- Badge disappears after first tap

### Home Screen Widget

**Layout:**
```
┌─────────────────────────┐
│   🔥 42 days            │
│   M  T  W  T  F         │
│   ✓  ✓  ○  ✓  ○         │
└─────────────────────────┘
```

**Theme Support:**
- Light mode: #F8F9FA background
- Dark mode: #121212 background
- System: Follows device preference

---

## Common Tasks

### Finding Design Specs
→ See [STREAK_WIDGET_DESIGN.md](./STREAK_WIDGET_DESIGN.md) - Section 1.3 "Design Specifications"

### Understanding Data Flow
→ See [STREAK_WIDGET_ARCHITECTURE.md](./STREAK_WIDGET_ARCHITECTURE.md) - Section "Data Flow"

### Getting Color Values
→ See [STREAK_WIDGET_DESIGN_REFERENCE.md](./STREAK_WIDGET_DESIGN_REFERENCE.md) - Section "Colors"

### Checking Icon Assets
→ See [STREAK_WIDGET_DESIGN_REFERENCE.md](./STREAK_WIDGET_DESIGN_REFERENCE.md) - Section "Icon Assets"

### Understanding State Management
→ See [STREAK_WIDGET_ARCHITECTURE.md](./STREAK_WIDGET_ARCHITECTURE.md) - Section "State Management"

---

## Widget Types

### 1. In-App Streak Circle
- **Location:** Home screen stats section
- **Purpose:** Display current streak or consistency score
- **Interaction:** Tap to view detailed stats
- **Animation:** Rotating gradient when active

### 2. Meditation Widget (Android Home Screen)
- **Purpose:** Show current streak with 5-day calendar
- **Data:** Streak count + meditation dates
- **Theme:** Adapts to app theme preference

### 3. Consistency Widget (Android Home Screen)
- **Purpose:** Show consistency percentage with 5-day calendar
- **Data:** Consistency score (0-100%)
- **Theme:** Adapts to app theme preference

---

## Dependencies

### Flutter
- `flutter_riverpod` - State management
- `medito/constants/icons/medito_icons.dart` - Icon assets
- `medito/constants/colors/color_constants.dart` - Colors

### Android
- `androidx.glance` - Widget framework (Jetpack Glance)
- `home_widget` - Flutter-Android bridge
- `androidx.compose` - UI composition

---

## Related Features

### Streak Freeze
Allows users to preserve their streak during breaks. Freeze dates are included in the calendar display.

**Implementation:** `/lib/views/home/widgets/bottom_sheet/stats/streak_freeze_suggestion_widget.dart`

### Display Toggle
Users can toggle between showing streak count and consistency percentage.

**Provider:** `/lib/providers/streak_circle_display_provider.dart`

---

## Testing

Test files are located in:
- `/test/integration/` - End-to-end tests
- `/test/stats_manager/` - Streak calculation tests
- `/test/widgets/` - Widget tests
- `/test/views/home/widgets/stats/` - Controller tests

See [STREAK_WIDGET_ARCHITECTURE.md](./STREAK_WIDGET_ARCHITECTURE.md) for complete test file listing.

---

## Contributing

When making changes to the streak widget:

1. Review the appropriate documentation file above
2. Make your changes following the existing patterns
3. Update the documentation if design values change
4. Run tests to ensure nothing breaks
5. Consider theme support (light/dark/system)

---

## Documentation Maintenance

These documentation files should be updated when:
- Design tokens change (colors, sizes, padding)
- New widget variants are added
- File locations change
- New dependencies are added
- Animation specifications change

---

**Last Updated:** Documentation created for current implementation
**Status:** ✅ Complete and verified

# Streak Widget Architecture

This document provides a visual overview of how the streak widget components are organized and interact.

## File Structure Tree

```
medito-app/
│
├── lib/
│   ├── views/home/widgets/stats/
│   │   ├── streak_circle.dart                    # Main Flutter widget
│   │   ├── streak_circle_controller.dart         # Business logic & animations
│   │   └── streak_circle_constants.dart          # Design constants (sizes, padding)
│   │
│   ├── views/home/widgets/bottom_sheet/stats/
│   │   └── streak_freeze_suggestion_widget.dart  # Streak freeze UI
│   │
│   ├── services/
│   │   └── home_widget_service.dart              # Bridge to Android widgets
│   │
│   ├── providers/
│   │   ├── stats_provider.dart                   # Stats data management
│   │   ├── streak_circle_provider.dart           # Badge state (seen/unseen)
│   │   └── streak_circle_display_provider.dart   # Display type (streak vs consistency)
│   │
│   └── constants/
│       └── styles/widget_styles.dart             # Shared widget styles
│
└── android/app/src/main/
    ├── kotlin/meditofoundation/medito/widget/
    │   ├── MeditationWidget.kt                   # Streak home screen widget
    │   ├── ConsistencyWidget.kt                  # Consistency home screen widget
    │   ├── MeditationWidgetReceiver.kt           # Widget update receiver
    │   └── ConsistencyWidgetReceiver.kt          # Widget update receiver
    │
    └── res/drawable/
        ├── streak_day_checked.xml                # White circle with purple checkmark
        ├── streak_day_checked_purple.xml         # Purple circle with white checkmark
        ├── streak_day_unchecked.xml              # Semi-transparent gray circle
        ├── ic_fire_purple.xml                    # Purple fire icon
        ├── ic_fire_grey.xml                      # Gray fire icon
        └── widget_streak_preview.png             # Widget preview image
```

## Component Interaction Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         User Interface                           │
└─────────────────────────────────────────────────────────────────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
                    ▼                         ▼
         ┌──────────────────┐      ┌──────────────────┐
         │  Flutter In-App  │      │ Android Home     │
         │  Streak Circle   │      │ Screen Widgets   │
         └──────────────────┘      └──────────────────┘
                    │                         │
                    │                         │
         ┌──────────┴──────────┐   ┌─────────┴─────────┐
         │                     │   │                   │
         ▼                     ▼   ▼                   ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│ streak_circle   │  │ Controller &    │  │ MeditationWidget│
│ .dart           │  │ Constants       │  │ .kt             │
└─────────────────┘  └─────────────────┘  └─────────────────┘
         │                     │                     │
         └──────────┬──────────┘                     │
                    │                                │
                    ▼                                │
         ┌──────────────────────┐                   │
         │   Providers Layer    │                   │
         ├──────────────────────┤                   │
         │ • stats_provider     │◄──────────────────┘
         │ • streak_circle      │          │
         │ • display_provider   │          │
         └──────────────────────┘          │
                    │                      │
                    ▼                      ▼
         ┌──────────────────────┐  ┌──────────────────┐
         │   Data Models        │  │ home_widget      │
         │ • LocalAllStats      │◄─┤ _service.dart    │
         │ • AudioCompleted     │  │                  │
         └──────────────────────┘  └──────────────────┘
                    │                      │
                    ▼                      ▼
         ┌──────────────────────┐  ┌──────────────────┐
         │   Local Storage      │  │ Android Shared   │
         │   (Flutter)          │  │ Preferences      │
         └──────────────────────┘  └──────────────────┘
```

## Data Flow

### In-App Widget Update Flow
```
1. User completes meditation
2. stats_provider updates LocalAllStats
3. streak_circle_controller recalculates:
   - isStreakDoneToday()
   - getDisplayValue()
   - getProgressValue()
4. Widget rebuilds with new state
5. Animation triggers if streak completed today
```

### Home Screen Widget Update Flow
```
1. User completes meditation
2. stats_provider updates LocalAllStats
3. home_widget_service.updateWidget() called
4. Data serialized to SharedPreferences:
   - streak_current
   - meditation_dates (JSON)
   - freeze_dates (JSON)
   - consistency_score
   - theme_preference
5. Broadcast sent to Android widgets
6. MeditationWidget/ConsistencyWidget reads data
7. Widget UI rebuilds on home screen
```

## Design Element Mapping

### In-App (Flutter)
```
StreakCircle Widget Structure:
┌─────────────────────────────────────┐
│ Container (gradient border if active)│
│ ┌───────────────────────────────────┐ │
│ │ Material (cardColor background)   │ │
│ │ ┌─────────────────────────────────┐ │ │
│ │ │ Row (padding: 11,9,13,9)        │ │ │
│ │ │ ┌────┐  ┌──────┐               │ │ │
│ │ │ │Icon│  │ Text │               │ │ │
│ │ │ │🔥  │  │  42  │               │ │ │
│ │ │ └────┘  └──────┘               │ │ │
│ │ └─────────────────────────────────┘ │ │
│ └───────────────────────────────────┘ │
└─────────────────────────────────────┘
      ↑ Animated gradient (3s rotation)
```

### Home Screen Widget (Android)
```
Widget Layout:
┌─────────────────────────────────────┐
│  Box (theme background, 8dp padding) │
│  ┌─────────────────────────────────┐│
│  │ Column (centered)               ││
│  │ ┌─────────────────────────────┐ ││
│  │ │ Row (Streak Display)        │ ││
│  │ │ [🔥] 42 days                │ ││
│  │ └─────────────────────────────┘ ││
│  │ ┌─────────────────────────────┐ ││
│  │ │ Row (Calendar - 5 days)     │ ││
│  │ │  M    T    W    T    F      │ ││
│  │ │  ✓    ✓    ○    ✓    ○      │ ││
│  │ └─────────────────────────────┘ ││
│  └─────────────────────────────────┘│
└─────────────────────────────────────┘
  ✓ = streak_day_checked_purple.xml
  ○ = streak_day_unchecked.xml
```

## Theme Colors

### Light Mode
```
Background:    #F8F9FA (lightBackground)
Text:          #000000 (black)
Inactive:      #E5E7EB (lightGrey)
Active:        #917DF0 (lightPurple)
```

### Dark Mode
```
Background:    #121212 (dark)
Text:          #FFFFFF (white)
Secondary:     #B3B3B3 (light grey)
Inactive:      #2C2C2C (dark grey)
Active:        #917DF0 (lightPurple)
```

## State Management

### Display Logic
```
Display Mode Decision:
  IF streak < 100 THEN
    show: Consistency Score (0-100%)
    icon: CircularProgressIndicator
  ELSE
    show: Streak Count
    icon: Fire Icon
  END IF

Visual State:
  IF completedToday THEN
    style: Bold text, animated gradient, purple icon
  ELSE
    style: Normal text, no animation, grey icon
  END IF
```

### Badge Logic
```
Badge Display Decision:
  IF NOT hasSeenStreakCircle THEN
    display: Amber badge (8dp) on top-left
    onTap: markAsSeen() + open stats
  ELSE
    onTap: open stats
  END IF
```

## File Dependencies

### Flutter Dependencies
- `flutter_riverpod` - State management
- `medito/constants/icons/medito_icons.dart` - Icon assets
- `medito/constants/colors/color_constants.dart` - Color values
- `medito/constants/styles/widget_styles.dart` - Typography

### Android Dependencies
- `androidx.glance` - Widget framework (Jetpack Glance)
- `home_widget` - Flutter-Android bridge
- `androidx.compose` - UI composition
- `org.json.JSONArray` - JSON parsing

## Related Features

### Streak Freeze
- Location: `/lib/views/home/widgets/bottom_sheet/stats/streak_freeze_suggestion_widget.dart`
- Purpose: Allows users to preserve streak during breaks
- Integration: Freeze dates included in widget calendar display

### Stats Management
- Location: `/lib/providers/stats_provider.dart`
- Data: Total tracks, completion times, streak history
- Storage: Local database (SQLite via Floor)

## Testing

### Test Files
```
test/
├── integration/
│   └── streak_freeze_flow_test.dart              # E2E streak freeze
├── stats_manager/
│   ├── basic_streak_test.dart                    # Streak calculation logic
│   └── streak_freeze_test.dart                   # Freeze functionality
├── providers/
│   └── streak_freeze_suggestion_provider_test.dart
├── widgets/
│   └── streak_freeze_widget_test.dart            # Widget tests
└── views/home/widgets/stats/
    └── streak_circle_controller_test.dart        # Controller tests
```

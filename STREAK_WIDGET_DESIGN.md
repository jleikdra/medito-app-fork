# Streak Widget Design Documentation

This document provides a comprehensive overview of the streak widget design and its implementation in the Medito app.

## Overview

The streak widget appears in two contexts:
1. **In-App Widget**: A circular widget displayed in the Flutter app
2. **Home Screen Widgets**: Android home screen widgets showing streak information

---

## 1. In-App Flutter Widget (StreakCircle)

### Location
- **Main Widget**: `/lib/views/home/widgets/stats/streak_circle.dart`
- **Controller**: `/lib/views/home/widgets/stats/streak_circle_controller.dart`
- **Constants**: `/lib/views/home/widgets/stats/streak_circle_constants.dart`

### Design Specifications

#### Visual Constants (from `streak_circle_constants.dart`)
```dart
borderRadius: 30.0 dp
iconSize: 20.0 dp
innerIconSize: 18.0 dp
fontSize: 16.0 sp
lineHeight: 1.4
padding: EdgeInsets.fromLTRB(11, 9, 13, 9)
animationDuration: 3 seconds
```

#### Color Scheme (from `streak_circle.dart`)
- **Active State** (when streak is done today):
  - Border: Animated gradient with `ColorConstants.lightPurple` (#917DF0)
  - Gradient stops: [0.1, 0.2, 0.5, 0.8, 0.9] with varying alpha values
  - Gradient animation: 3-second rotation
  - Icon: Purple fire (`ColorConstants.lightPurple`)
  - Text: Bold weight

- **Inactive State**:
  - Border: None
  - Icon: Gray fire (theme's onSurface color)
  - Text: Normal weight

#### Layout Structure
```
Container (with optional animated gradient border)
└─ Material with InkWell
   └─ Ink container (theme's cardColor, 30dp border radius)
      └─ Row (padding: 11, 9, 13, 9)
         ├─ Icon (fire or circular progress)
         │  - Fire icon for streak display
         │  - Circular progress for consistency score
         ├─ Spacer (8dp)
         └─ Text (streak count or consistency %)
```

#### Behavior
- **Badge**: Shows amber badge (8dp) when user hasn't seen the streak circle yet
- **Display Toggle**: Shows either:
  - **Streak Count**: When streak >= 100 or displayType is `currentStreak`
  - **Consistency Score**: When streak < 100 (with circular progress indicator)
- **Animation**: Rotating gradient border when streak is completed today
- **Tap Action**: Opens stats details

### Related Providers
- **Stats Provider**: `/lib/providers/stats_provider.dart`
- **Streak Circle Provider**: `/lib/providers/streak_circle_provider.dart`
- **Streak Circle Display Provider**: `/lib/providers/streak_circle_display_provider.dart`

---

## 2. Android Home Screen Widgets

### Widget Implementations

#### A. Meditation Widget (Streak-based)
**Location**: `/android/app/src/main/kotlin/meditofoundation/medito/widget/MeditationWidget.kt`

**Design Specifications**:
- **Background**: Theme-aware (light/dark mode)
  - Light mode: #F8F9FA (lightBackground)
  - Dark mode: #121212
- **Layout**: Vertical centered column
  - Streak display row (fire icon + count + label)
  - Calendar row (5 days of activity indicators)
- **Fire Icon**: 24x24 dp
  - Purple (#917DF0) when active today
  - Gray when inactive
- **Text**: 
  - Streak count: 24sp, bold
  - Label: 14sp, normal
  - Day abbreviations: 9sp
- **Activity Indicators**: 20x20 dp circles
  - Completed: Purple checkmark on purple circle
  - Incomplete: Semi-transparent gray circle

#### B. Consistency Widget
**Location**: `/android/app/src/main/kotlin/meditofoundation/medito/widget/ConsistencyWidget.kt`

**Design Specifications**:
- Same layout as Meditation Widget
- Shows consistency percentage instead of streak count
- Same theme-aware colors
- Same activity calendar indicators

### Widget Drawables

#### Location
`/android/app/src/main/res/drawable/`

#### Files
1. **streak_day_checked.xml**
   - White oval background
   - Purple checkmark overlay
   - Used for active days in light mode

2. **streak_day_checked_purple.xml**
   - Purple circle (#917DF0) - 24x24 dp
   - White checkmark overlay
   - Used for active days in widgets

3. **streak_day_unchecked.xml**
   - Semi-transparent white oval (#40FFFFFF)
   - Used for inactive days

4. **ic_fire_purple.xml** & **ic_fire_grey.xml**
   - Fire icon in purple and gray variants
   - Used to indicate active/inactive streak status

5. **widget_streak_preview.png**
   - Preview image for widget picker (33 KB)

### Widget Service
**Location**: `/lib/services/home_widget_service.dart`

**Data Keys**:
- `streak_current`: Current streak count
- `meditation_dates`: JSON array of meditation timestamps
- `freeze_dates`: JSON array of freeze usage dates
- `day_label`: Localized "day" text
- `days_label`: Localized "days" text
- `total_tracks_completed`: Total completed tracks
- `consistency_score`: Consistency percentage (0-100)
- `theme_preference`: Theme setting (light/dark/system)

---

## 3. Design Patterns

### Color System
- **Primary Purple**: #917DF0 (`ColorConstants.lightPurple`)
- **Active Indicator**: Purple checkmark on purple circle
- **Inactive Indicator**: Semi-transparent gray
- **Amber Badge**: Used for "new feature" notifications

### Theme Support
Both in-app and widget designs support:
- Light mode
- Dark mode
- System theme preference

### Animation
- **In-App Only**: 3-second rotating gradient border when streak is active
- **Widgets**: Static design (no animations)

### Calendar Display
- Shows last 5 days (oldest to newest, left to right)
- Day abbreviations: S, M, T, W, T, F, S
- Activity indicators show meditation + freeze usage days

---

## 4. User Experience

### Visual Feedback
1. **Completed Today**: 
   - Purple glowing gradient border (in-app)
   - Purple fire icon
   - Bold text
   
2. **Not Completed Today**:
   - No border
   - Gray fire icon
   - Normal text weight

3. **First Time User**:
   - Amber badge on top-left
   - Badge removed after first tap

### Consistency Score vs Streak
- **Streak < 100**: Shows consistency percentage with circular progress
- **Streak >= 100**: Shows streak count with fire icon
- User can manually toggle between displays via settings

---

## Summary

The streak widget design is located across multiple files:

**Flutter (In-App)**:
- `/lib/views/home/widgets/stats/streak_circle.dart` (main widget)
- `/lib/views/home/widgets/stats/streak_circle_controller.dart` (logic)
- `/lib/views/home/widgets/stats/streak_circle_constants.dart` (design specs)

**Android (Home Screen)**:
- `/android/app/src/main/kotlin/meditofoundation/medito/widget/MeditationWidget.kt`
- `/android/app/src/main/kotlin/meditofoundation/medito/widget/ConsistencyWidget.kt`
- `/android/app/src/main/res/drawable/streak_day_*.xml` (activity indicators)
- `/android/app/src/main/res/drawable/ic_fire_*.xml` (fire icons)

**Service Layer**:
- `/lib/services/home_widget_service.dart` (data bridge)

**Key Design Values**:
- Border radius: 30dp
- Purple accent: #917DF0
- Icon size: 20dp (in-app), 24dp (widget)
- Padding: 11, 9, 13, 9 (in-app)
- Calendar indicators: 20x20 dp circles

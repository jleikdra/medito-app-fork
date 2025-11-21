# Streak Widget Design Quick Reference

> Quick reference guide for designers working on the Medito app streak widget

## Design Token Values

### Spacing & Dimensions

#### In-App Widget (Flutter)
| Property | Value | Notes |
|----------|-------|-------|
| Border Radius | 30 dp | Main container corner radius |
| Icon Size | 20 dp | Fire icon outer size |
| Inner Icon Size | 18 dp | Fire icon inner size |
| Font Size | 16 sp | Streak count/percentage text |
| Line Height | 1.4 | Text line height multiplier |
| Padding | 11, 9, 13, 9 dp | Left, Top, Right, Bottom |
| Badge Size | 8 dp | "New" indicator badge |
| Spacer Width | 8 dp | Gap between icon and text |

#### Home Screen Widget (Android)
| Property | Value | Notes |
|----------|-------|-------|
| Container Padding | 8 dp | Overall widget padding |
| Fire Icon | 24 × 24 dp | Streak indicator icon |
| Icon Top Padding | 2 dp | Alignment adjustment |
| Icon End Padding | 4 dp | Space after fire icon |
| Streak Text | 24 sp bold | Main number display |
| Label Text | 14 sp normal | "days" or "%" label |
| Day Label Text | 9 sp | Calendar day letters |
| Activity Circle | 20 × 20 dp | Display size (intrinsic: 24 × 24 dp) |
| Spacer (after streak) | 4 dp | Gap between sections |
| Spacer (after label) | 6 dp | Consistency widget gap |
| Spacer (day to circle) | 2 dp | Calendar spacing |

### Colors

#### Brand Colors
```
Primary Purple (lightPurple): #917DF0  (ColorConstants.lightPurple from color_constants.dart)
Badge Color:                  #EF5E55  (ColorConstants.amber from color_constants.dart)
```

Note: Despite the variable name "amber", the actual color is a coral/red shade.

#### Light Theme
```
Background:          #F8F9FA
Text:                #000000
Secondary Text:      #000000
Inactive Circle:     #E5E7EB
Checkmark:           #917DF0
```

#### Dark Theme
```
Background:          #121212
Text:                #FFFFFF
Secondary Text:      #B3B3B3
Inactive Circle:     #2C2C2C
Checkmark:           #917DF0
```

#### Alpha Values (In-App Gradient)
```
Gradient Stop 1:     lightPurple @ 20% opacity (0.2)
Gradient Stop 2:     lightPurple @ 35% opacity (0.35)
Gradient Stop 3:     lightPurple @ 100% opacity (1.0)
Gradient Stop 4:     lightPurple @ 30% opacity (0.3)
Gradient Stop 5:     lightPurple @ 25% opacity (0.25)
Gradient Positions:  [0.1, 0.2, 0.5, 0.8, 0.9]
```

## Animation Specifications

### In-App Widget
| Animation | Duration | Type | Trigger |
|-----------|----------|------|---------|
| Gradient Rotation | 3 seconds | Continuous loop | When streak completed today |
| Gradient Transform | 360° (2π rad) | Rotation | Full rotation per cycle |

### Home Screen Widget
No animations (static design)

## State Variants

### In-App Widget States

#### 1. Active (Completed Today)
- **Border**: Animated gradient (rotating purple gradient)
- **Fire Icon**: Purple (#917DF0)
- **Text**: Bold weight
- **Background**: Theme card color
- **Behavior**: Gradient rotates continuously

#### 2. Inactive (Not Completed Today)
- **Border**: None
- **Fire Icon**: Gray (theme onSurface)
- **Text**: Normal weight
- **Background**: Theme card color
- **Behavior**: Static

#### 3. First Time User
- **Additional Element**: Amber badge (8dp, top-left)
- **Badge Position**: Alignment.topLeft
- **Badge Color**: ColorConstants.amber
- **Behavior**: Badge disappears after first tap

#### 4. Consistency Score Mode (streak < 100)
- **Icon**: Circular progress indicator instead of fire
- **Progress Stroke**: 2dp, rounded caps
- **Progress Color**: Purple (active) or gray (inactive)
- **Background Circle**: onSurface @ 20% opacity
- **Text**: Percentage with "%" suffix

### Home Screen Widget States

#### 1. Active Today
- **Fire Icon**: ic_fire_purple.xml
- **Today Circle**: streak_day_checked_purple.xml
- **Text**: Bold

#### 2. Inactive Today
- **Fire Icon**: ic_fire_grey.xml
- **Today Circle**: streak_day_unchecked.xml
- **Text**: Normal weight

## Icon Assets

### In-App (Flutter)
- Fire icon path: `MeditoIcons.fire`
- Help icon path: `MeditoIcons.help` (error state)

### Home Screen (Android)
| File | Size (Intrinsic / Display) | Usage |
|------|-------------------------------|-------|
| ic_fire_purple.xml | 24 × 24 dp | Active fire icon |
| ic_fire_grey.xml | 24 × 24 dp | Inactive fire icon |
| streak_day_checked_purple.xml | 24 × 24 dp / 20 × 20 dp | Completed day (purple circle + white check) |
| streak_day_checked.xml | - | Light mode variant (white + purple check) |
| streak_day_unchecked.xml | - | Incomplete day (semi-transparent gray) |
| ic_checkmark_purple.xml | - | Purple checkmark overlay |
| ic_checkmark_white.xml | - | White checkmark overlay |

## Layout Specifications

### In-App Widget Hierarchy
```
Container (conditional gradient border, 2dp padding when active)
└─ Material (transparent, ripple effect)
   └─ InkWell (30dp border radius, tappable)
      └─ Ink (cardColor background, 30dp border radius)
         └─ Padding (11, 9, 13, 9)
            └─ Row (centered, min size)
               ├─ Icon/Progress (20 × 20 dp)
               ├─ SizedBox (8dp width)
               └─ Text (16sp)
```

### Home Screen Widget Hierarchy
```
Box (fillMaxSize, theme background, 8dp padding, tappable)
└─ Column (fillMaxSize, centered vertically & horizontally)
   ├─ Row (fillMaxWidth, centered - Streak Display)
   │  ├─ Image (24 × 24 dp, 2dp top + 4dp end padding)
   │  ├─ Text (24sp bold - count)
   │  ├─ Spacer (2dp)
   │  └─ Text (14sp - label)
   ├─ Spacer (4dp or 6dp)
   └─ Row (fillMaxWidth, centered - Calendar)
      └─ [5 × Column (equal weight, centered)]
         ├─ Text (9sp - day letter)
         ├─ Spacer (2dp)
         └─ Image (20 × 20 dp - activity indicator)
```

## Typography

### In-App
| Element | Font Size | Weight | Family | Line Height |
|---------|-----------|--------|--------|-------------|
| Streak Count | 16 sp | Bold (active) / Normal (inactive) | DM Mono | 1.4 |
| Consistency % | 16 sp | Bold (active) / Normal (inactive) | DM Mono | 1.4 |

### Home Screen
| Element | Font Size | Weight | Notes |
|---------|-----------|--------|-------|
| Streak Count | 24 sp | Bold | Main display number |
| Label Text | 14 sp | Normal | "day", "days", or "%" |
| Day Letters | 9 sp | Normal | S, M, T, W, T, F, S |

## Interaction Patterns

### In-App Widget
| State | Tap Action | Visual Feedback |
|-------|-----------|-----------------|
| First time | Open stats + remove badge | Ripple effect |
| Subsequent | Open stats sheet | Ripple effect |
| Any | Material InkWell ripple | Circular ripple from tap point |

### Home Screen Widget
| Action | Result |
|--------|--------|
| Tap anywhere | Launch Medito app (MainActivity) |

## Accessibility

### Content Descriptions
- **Fire Icon**: "Fire icon" (state-aware)
- **Help Icon**: "Help" (error state)
- **Completed Day**: "Completed day"
- **Empty Day**: "Empty day"

## Design Files Location

### Source Files
```
Flutter:   /lib/views/home/widgets/stats/
Android:   /android/app/src/main/kotlin/meditofoundation/medito/widget/
Drawables: /android/app/src/main/res/drawable/
```

### Documentation
```
Design Specs:     STREAK_WIDGET_DESIGN.md
Architecture:     STREAK_WIDGET_ARCHITECTURE.md
Quick Reference:  STREAK_WIDGET_DESIGN_REFERENCE.md (this file)
```

## Common Design Patterns

### Calendar Day Indicators
- **Pattern**: Duolingo-style streak visualization
- **Display**: Show 5 most recent days (oldest left, newest right)
- **Day Letters**: Single character abbreviations
- **Visual Hierarchy**: Letter above indicator circle

### Status Communication
- **Color Coding**: Purple = active/completed, Gray = inactive
- **Weight**: Bold = active state, Normal = inactive state
- **Animation**: Movement = active (in-app only)

### Theme Consistency
- **Light Mode**: High contrast, crisp edges
- **Dark Mode**: Reduced brightness, same purple accent
- **System**: Follows device preference

## Edge Cases

### Display Logic
- Streak = 1: Show "day" (singular)
- Streak ≠ 1: Show "days" (plural)
- Streak < 100: Show consistency score by default
- Streak >= 100: Show streak count
- User override: Can force streak display via settings

### Error States (In-App)
- **Loading**: Gray shimmer with spinner (16 × 16 dp)
- **Error with data**: Show last known data
- **Error without data**: Show help icon with gray background
- **Tap on error**: Refresh stats

### Data Sync
- **Missing data**: Falls back to empty array `[]`
- **Invalid JSON**: Returns empty list
- **Theme preference**: Defaults to "system" if not set

## Measurement Units

- **dp**: Density-independent pixels (Android/Flutter)
- **sp**: Scale-independent pixels (text)
- **rad**: Radians (animation rotation)
- **Opacity**: 0.0 (transparent) to 1.0 (opaque)
- **Alpha**: 0 to 255 (0 = transparent, 255 = opaque)

## Version Notes

This design reference documents the current implementation of the streak widget.

### Recent Changes
- Theme-aware colors for widgets (light/dark/system)
- 5-day calendar display (previously 7 days)
- Consistency score with circular progress indicator
- Streak freeze integration in calendar

### Future Considerations
- iOS widget support (currently Android only)
- Custom animation speeds
- Configurable calendar day count
- Enhanced accessibility features

# 07 UI DESIGN SYSTEM & NAVIGATION – FULL DETAILED SPEC

## 7.1 Core UI Philosophy (Strict)

**Default iOS first.**

Language Skull must feel like a native Apple app that happens to have a beautiful dark gothic theme. We do **not** fight the system. We use:

- `NavigationStack` + large titles
- `TabView` with default tab bar
- Native sheets, alerts, and action sheets
- Liquid Glass / `.ultraThinMaterial` and `.regularMaterial` extensively
- Default gestures (swipe back, long press, etc.)
- SF Symbols for all icons

Only the color palette and very subtle skull motif differentiate us from a standard dark iOS app.

## 7.2 Color Palette (Exact Hex Values)

- Background: `#050505`
- Surface / Card: `#111111` + material blur
- Accent (buttons, highlights, progress): `#4A1C1C` (muted burgundy)
- Subtle highlight / parchment: `#D4C4A8`
- Text Primary: `#FFFFFF`
- Text Secondary: `#E5E5E5`

## 7.3 Header Rule (Non-Negotiable)

**No skull logo in the navigation bar.**

Right side of the navigation bar = circular User Avatar (SF Symbol `person.circle.fill` or user photo).

Tapping the avatar opens a native menu with exactly these items:
1. Profile
2. Manage Plan (full billing + subscription info)
3. Refer a Friend
4. Sign Out

## 7.4 Tab Bar

Bottom `TabView` with 4 tabs:
- Home (skull icon)
- Calendar
- Admin (visible in MVP, easy to hide before submission)
- Profile

Use default iOS tab bar appearance with our accent color for selected state.

## 7.5 Cursor Implementation Rules for UI

- Every screen must be built with default SwiftUI components first.
- Only apply our color palette via a custom `Color` extension or `Theme` environment object.
- Use `.background(.ultraThinMaterial)` liberally on cards and sheets.
- Add very subtle skull motif only in empty states, loading screens, and the app icon.
- Do **not** create custom button styles or heavy visual overrides until a Figma file is provided later.

This approach guarantees we ship a clean, native-feeling app that passes App Review easily and feels premium.
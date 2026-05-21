# 07 UI DESIGN SYSTEM & NAVIGATION – FULL DETAILED END-TO-END SPEC

## 7.1 Core UI Philosophy (Strict – Default iOS First)

**Language Skull must feel like a native, premium Apple app that happens to have a beautiful dark gothic theme.**

We do **not** fight the iOS system. We embrace it.

Rules:
- Use `NavigationStack` + large titles by default
- Use `TabView` with the standard bottom tab bar
- Use native sheets, alerts, confirmation dialogs, and action sheets
- Use liquid glass / `.ultraThinMaterial` and `.regularMaterial` extensively on cards, sheets, and backgrounds
- Support all default gestures (swipe back, long press for context menus, pull-to-refresh where appropriate)
- Use SF Symbols for every icon
- Keep animations subtle and system-like (`.easeInOut`, `.spring`)

Only the color palette and very subtle skull motif differentiate us from a standard high-quality dark iOS app. Heavy custom styling is **forbidden** until a Figma file is provided later.

## 7.2 Exact Color Palette (Use These Values)

- Background (main view): `#050505`
- Surface / Card background: `#111111` + material blur effect
- Accent color (buttons, progress rings, highlights, active tab): `#4A1C1C` (muted burgundy)
- Subtle parchment / secondary highlight: `#D4C4A8`
- Text Primary: `#FFFFFF`
- Text Secondary / captions: `#E5E5E5`

Apply colors via a `Theme` environment object or simple `Color` extension so they are easy to change later.

## 7.3 Header / Navigation Bar Rule (Non-Negotiable)

**No skull logo in the navigation bar.**

Right side of every navigation bar = circular User Avatar (use `person.circle.fill` SF Symbol or user photo if available).

Tapping the avatar must open a **native menu** (using `.contextMenu` or `Menu` with `Button`s) containing exactly these items in this order:

1. **Profile** – Shows user info, current streak, words/phrases learned, current plan name
2. **Manage Plan** – Full billing info, trial/subscription status, next billing date, cancel/upgrade options
3. **Refer a Friend** – Opens share sheet with pre-filled message + referral link
4. **Sign Out** – Signs out Apple ID and returns to guest/launch state

## 7.4 Tab Bar

Bottom `TabView` with exactly 4 tabs:
- **Home** (skull icon or house)
- **Calendar** (calendar icon)
- **Admin** (gear or wrench icon) – visible in MVP, easy to hide before submission
- **Profile** (person icon)

Use default iOS tab bar appearance. Selected tab uses our accent color (`#4A1C1C`).

## 7.5 Liquid Glass & Material Usage

Apply `.background(.ultraThinMaterial)` or `.background(.regularMaterial)` on:
- Cards and list rows
- Sheet backgrounds
- Tab bar and navigation bar when appropriate

This gives the premium "glassmorphism" feel while staying fully native.

## 7.6 When Custom Styling Is Allowed

**Only after a Figma file is provided.**

Until then:
- No custom button styles
- No heavy shadows or complex gradients
- No custom fonts (use system fonts)
- No custom navigation bar appearances beyond color
- Subtle skull motif only in empty states, loading screens, and the app icon

## 7.7 Accessibility & Polish Requirements

- All interactive elements must have proper accessibility labels and hints.
- Use dynamic type support (`.font(.body)` etc. scale correctly).
- Maintain sufficient contrast with the dark gothic palette.
- Add subtle haptic feedback on important actions (Mark as Done, session complete).
- Every screen must have a working SwiftUI Preview showing multiple states (loading, empty, populated, error).

## 7.8 Cursor Implementation Rules

- Start every screen with default SwiftUI components.
- Create a lightweight `Theme` struct or environment object for colors.
- Wrap all views in a `LanguageSkullTheme` modifier or similar.
- Do **not** create custom `ButtonStyle` or heavy visual overrides in MVP.
- Use SF Symbols exclusively for icons.
- Make the avatar menu using native `Menu` + `Button` or `.contextMenu`.
- Ensure the Admin tab can be hidden with a simple feature flag before App Store submission.

This approach guarantees we ship a clean, native-feeling, App Review-friendly app that still feels premium and on-brand.
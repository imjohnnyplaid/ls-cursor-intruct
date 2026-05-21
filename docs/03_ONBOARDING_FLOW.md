# 03 ONBOARDING FLOW – Detailed Specification (No Forced Signup)

## 3.1 Philosophy
Gradual, transparent onboarding inspired by the best native apps. Users must be able to try the app immediately as a guest before any signup. Apple Sign In is encouraged but optional.

## 3.2 Exact User Flow (Implement Precisely)

1. **Launch Screen**
   - Gothic skull logo with subtle parchment glow animation.
   - "Begin Training" button → enters guest mode immediately.

2. **Quick Guest Demo (Optional but Recommended)**
   - One short activity (e.g. New Words two-column list for a sample language).
   - After completion or skip: Proceed to full onboarding.

3. **Onboarding Steps (All in native sheets / NavigationStack)**
   - **Step 1**: First name (optional text field, "Continue as Guest" option).
   - **Step 2**: Choose target language – 5 locale-aware options (use `Locale.current` to prioritize, e.g. English device → Spanish/ French/ German/ Italian/ Portuguese). Flags + native names.
   - **Step 3**: Proficiency level – Beginner / Know Basic Words / Intermediate / Advanced / Fluent.
   - **Step 4**: Request notification permission (morning reminder only) with clear explanation.
   - **Step 5**: Apple Sign In button (prominent) + "Continue as Guest" smaller option.

4. **Post-Onboarding**
   - Create `UserProfile` in SwiftData.
   - Seed initial course content for chosen language.
   - Determine starting day based on proficiency (e.g. Beginner = Day 1, Intermediate = Day 3, etc.).
   - Navigate directly to Home/Dashboard showing Today’s Morning session.

5. **Guest → Signed-In Transition**
   - When user signs in later: Merge local guest progress into Apple-linked profile without data loss.

## 3.3 Edge Cases
- User skips everything → still gets a functional guest experience with limited languages.
- Device locale changes → re-offer language selection in Profile.
- Notification permission denied → still allow app usage + gentle re-prompt later in Profile.

## 3.4 UI Requirements
- All screens use default iOS dark style + gothic palette.
- Progress indicator at top of onboarding flow.
- Back gestures fully supported.

**Cursor Instruction**: Implement this flow completely with smooth transitions, proper SwiftData saving, and guest mode support. Test full path from cold launch to first study session.

All rules in this file are binding.
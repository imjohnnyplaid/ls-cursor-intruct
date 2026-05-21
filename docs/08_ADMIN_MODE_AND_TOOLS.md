# 08 ADMIN MODE – FULL DETAILED SPEC

## 8.1 Visibility in MVP

Admin mode is **visible by default** during development.
A clear toggle exists in Settings (or a long-press on the Admin tab icon) to hide it completely before App Store submission.

## 8.2 Admin Tab Contents (MVP)

1. **Content Management**
   - Import new language folder (JSON + Markdown)
   - View / edit existing words and phrases (basic list + form)
   - Re-generate or update Study Plan from template

2. **Study Plan Editor** (Form-based for MVP)
   - Select day + Morning/Evening
   - Add / remove / reorder activities
   - Simple drag-and-drop list (or up/down buttons)

3. **Preview as Learner**
   - Button that switches the app into "learner view" using the currently selected course and plan.
   - Useful for quickly testing new content or plan changes.

## 8.3 Cursor Requirements

- Create an `AdminFeatureFlag` (simple `@AppStorage` or environment value).
- All admin UI must be wrapped in `if isAdminMode { ... }`.
- Provide a clear "Hide Admin Mode for App Store" action that sets the flag to false and shows a confirmation.
- Keep the admin UI functional but not overly polished (we can improve it after the core learner experience is solid).
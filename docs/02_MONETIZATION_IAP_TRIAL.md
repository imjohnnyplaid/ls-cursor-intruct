# 02 MONETIZATION, IAP & TRIAL – Full Production Specification

## 2.1 Business Rules
- App is **free to download**.
- New users get a **7-day full Pro trial** (unlimited languages, all features, admin mode, etc.).
- After trial ends: Monthly auto-renewable subscription required for continued Pro access.
- No other IAPs in MVP (single monthly plan only).
- Referral: "You and your friend both get 3 months free" (MVP UI + stub tracking).

## 2.2 Technical Implementation (StoreKit 2)
- Use **StoreKit 2** exclusively.
- Define one auto-renewable subscription product in App Store Connect (monthly).
- Implement:
  - Trial eligibility check on launch.
  - Receipt validation (local + server if needed later).
  - Grace period handling.
  - Restore Purchases functionality.
- Store subscription status in SwiftData (`SubscriptionStatus` model) synced with receipt.

## 2.3 User Experience & Flows
- **Trial End Detection**: On app launch or when accessing locked features, show gentle contextual paywall.
- **Paywall Design**: Value-first (benefit bullets, price, "Start Free Trial" prominent button, "Restore Purchases", Privacy Policy link). Use native `SubscriptionStoreView` where appropriate, with custom dark gothic styling.
- **Manage Plan Screen** (accessible from avatar menu):
  - Current plan status (Trial / Active / Expired)
  - Price and billing cycle
  - Next billing date
  - Days remaining in trial (if applicable)
  - Cancel subscription button
  - Upgrade / Change plan options
  - Referral button

## 2.4 Edge Cases & Error Handling
- Trial expires while app is backgrounded → lock Pro features on next launch with clear message.
- No internet → show cached status + offline-friendly messaging.
- Failed purchase → graceful error with retry + contact support.
- Family Sharing / multiple devices → proper receipt handling.

## 2.5 Deliverables Needed from You
- Apple Developer Account active
- App Store Connect app record created
- Monthly subscription product ID created
- Test flight internal testers ready

**Cursor Instruction**: Implement full StoreKit 2 flow with receipt validation. Do not use fake/hard-coded subscription logic after seeding. Test trial expiry flow in Simulator (you can adjust device date for testing).

All rules in this file are binding for the entire build.
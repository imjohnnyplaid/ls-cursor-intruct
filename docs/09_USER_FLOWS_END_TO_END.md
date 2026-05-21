# 09 END-TO-END USER FLOWS – COMPLETE DETAILED SPEC

## 9.1 First Launch Flow (Complete Path)

1. Cold launch → Show splash with skull logo.
2. Check if user profile exists in SwiftData.
3. If no profile → Start Onboarding (see 03_ONBOARDING_FLOW.md).
4. If profile exists but no active course → Seed default course + show language selection.
5. If everything exists → Go directly to Home/Dashboard.

## 9.2 Daily Usage Flow (Morning Example)

1. User opens app in the morning.
2. Home shows "Good morning" + Today’s Morning progress ring.
3. User taps Morning card.
4. Sees linear list of activities with "Mark as Done" buttons.
5. Completes activities one by one.
6. After last activity → Confetti + "Great work! Evening session recommended."
7. Home updates progress ring.

## 9.3 Trial Expiry Flow

1. On launch, `SubscriptionManager` checks trial end date.
2. If trial has ended and user is not subscribed → Show contextual paywall (value-first, not aggressive).
3. User can still access previously unlocked content but cannot start new days or access new languages.
4. Clear messaging: "Your 7-day trial has ended. Subscribe to continue your progress."

## 9.4 Referral Flow (MVP Stub)

1. User taps "Refer a Friend" in avatar menu.
2. Native share sheet appears with pre-filled message + unique referral link.
3. Link contains a referral code (generated and stored locally).
4. When recipient installs and opens the app for the first time, the app reads the link (via onOpenURL or Universal Links).
5. Referral code is stored in `UserProfile.referredBy`.
6. On successful subscription, a local flag is set that the user is eligible for 3 months free (stub).

**Note**: Full automatic application of the 3-month credit happens in Phase 2.
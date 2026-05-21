# 06 STUDY PLAN & ACTIVITIES – FULL DETAILED END-TO-END SPEC

## 6.1 Overall Philosophy

The study plan is the heart of Language Skull. It must feel like a real coach wrote it — clear, repeatable, and progressive. Every user on the same course sees the exact same plan on the same day. This removes decision fatigue and creates consistency.

## 6.2 Core Data Flow

1. Admin defines a `StudyPlan` (in code or via future admin UI).
2. `StudyPlan` contains an array of `WeekPlan` or direct `DayPlan` objects.
3. Each `DayPlan` has `morningActivities` and `eveningActivities` arrays of `ActivityDefinition`.
4. On any given day, the app calculates the current day number for the user.
5. It assembles the list of activities for Morning and Evening.
6. User completes activities → `UserActivityCompletion` records are created.
7. Progress is shown on Home and Calendar.

## 6.3 ActivityDefinition (Detailed)

```swift
struct ActivityDefinition: Identifiable, Codable {
    let id: String
    let type: ActivityType
    let metadata: [String: String]   // e.g. ["direction": "EF", "sectionNumber": "3"]
    let order: Int
}
```

**Important**: `metadata` is a flexible dictionary. This allows the same activity type to carry different parameters without creating dozens of subclasses.

## 6.4 ActivityType Enum (Complete List for MVP)

```swift
enum ActivityType: String, Codable {
    // === WORDS ===
    case newWordsList                    // Two-column list
    case newWordsFlashcardsEF            // English → Foreign
    case newWordsFlashcardsFE            // Foreign → English
    case revisionWordsD1                 // Previous day only
    case revisionWordsD3                 // Last 3 days combined, original order

    // === PHRASES ===
    case newPhrasesList
    case newPhrasesFlashcardsEF
    case newPhrasesFlashcardsFE
    case revisionPhrasesD1
    case revisionPhrasesD3

    // === GRAMMAR ===
    case grammarParagraph                // Shows GrammarSection.number == X
}
```

## 6.5 How Revision D-3 Must Work (Critical Rule)

When an activity of type `revisionWordsD3` or `revisionPhrasesD3` is shown:

1. Collect **all** words/phrases where `dayIntroduced` is between (currentDay - 3) and (currentDay - 1).
2. Sort them **strictly by dayIntroduced ascending**, then by original order within the same day.
3. Present them as one continuous flashcard session (no shuffling).
4. User goes through the entire set once.

This preserves the "original learning order" requirement from the product vision.

## 6.6 Grammar Flow (Detailed)

- Grammar activities are **sequential**.
- The first grammar activity in the plan is usually `grammarParagraph` with metadata `["sectionNumber": "1"]`.
- When the user completes it, the app records that section 1 is done.
- The next grammar activity automatically uses section 2, and so on.
- If the user jumps to a past day, they see the grammar section that was scheduled for that day.

## 6.7 Daily Session Assembly Logic (Pseudocode for Cursor)

```swift
func getSession(for day: Int, timeOfDay: TimeOfDay) -> [ActivityDefinition] {
    let dayPlan = studyPlan.dayPlans.first { $0.dayNumber == day % 7 } ?? studyPlan.dayPlans[0]
    
    let activities = (timeOfDay == .morning) ? dayPlan.morningActivities : dayPlan.eveningActivities
    
    return activities.sorted { $0.order < $1.order }
}
```

## 6.8 Cursor Implementation Requirements

- Create a `StudyPlanEngine` service (actor recommended).
- It must be able to answer: "What are today’s Morning activities?" and "What are today’s Evening activities?"
- It must correctly handle D-3 revision ordering.
- It must support progressive multi-week plans (not just repeating 7-day cycle).
- All activity completion must be recorded with timestamp and linked to the specific `ActivityDefinition.id` + date.

This engine must be rock solid. Everything else depends on it.
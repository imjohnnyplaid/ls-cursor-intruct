# 04 DATA MODELS – Complete SwiftData Schema (Production Grade)

## 4.1 Core Principles
- All models use `@Model` (SwiftData).
- Strict separation: Raw immutable content vs User-specific progress.
- Proper `@Relationship` with `inverse` and `deleteRule`.
- Unique constraints on IDs.
- Support for future migrations via versioning.
- All models must be fully Codable where appropriate for JSON seeding.

## 4.2 Full Model Definitions (Implement Exactly)

```swift
import SwiftData
import Foundation

// MARK: - Enums
enum ProficiencyLevel: String, Codable, CaseIterable {
    case beginner, basic, intermediate, advanced, fluent
}

enum ActivityType: String, Codable {
    // Words
    case newWordsList
    case newWordsFlashcardsEF
    case newWordsFlashcardsFE
    case revisionWordsD1
    case revisionWordsD3
    
    // Phrases
    case newPhrasesList
    case newPhrasesFlashcardsEF
    case newPhrasesFlashcardsFE
    case revisionPhrasesD1
    case revisionPhrasesD3
    
    // Grammar
    case grammarParagraph
}

// MARK: - User & Profile
@Model
final class UserProfile {
    @Attribute(.unique) var id: String                    // UUID or Apple User ID
    var firstName: String?
    var appleUserID: String?                              // nil = guest
    var createdAt: Date
    
    var targetLanguage: String
    var proficiencyLevel: ProficiencyLevel
    var notificationEnabled: Bool = false
    
    // Relationships
    @Relationship(deleteRule: .cascade, inverse: \SubscriptionStatus.user)
    var subscriptionStatus: SubscriptionStatus?
    
    @Relationship(deleteRule: .cascade, inverse: \UserActivityCompletion.user)
    var completions: [UserActivityCompletion] = []
    
    init(id: String = UUID().uuidString, 
         firstName: String? = nil, 
         appleUserID: String? = nil, 
         targetLanguage: String, 
         proficiencyLevel: ProficiencyLevel) {
        self.id = id
        self.firstName = firstName
        self.appleUserID = appleUserID
        self.createdAt = Date()
        self.targetLanguage = targetLanguage
        self.proficiencyLevel = proficiencyLevel
    }
}

// MARK: - Subscription
@Model
final class SubscriptionStatus {
    var isPro: Bool = false
    var trialEndDate: Date?
    var subscriptionEndDate: Date?
    var lastReceiptValidationDate: Date?
    
    @Relationship(inverse: \UserProfile.subscriptionStatus)
    var user: UserProfile?
    
    init() {}
}

// MARK: - Content Models
@Model
final class Course {
    @Attribute(.unique) var id: String
    var name: String
    var foreignLanguage: String
    var isUnlocked: Bool = false
    var contentVersion: Int = 1
    
    @Relationship(deleteRule: .cascade, inverse: \Word.course)
    var words: [Word] = []
    
    @Relationship(deleteRule: .cascade, inverse: \Phrase.course)
    var phrases: [Phrase] = []
    
    @Relationship(deleteRule: .cascade, inverse: \GrammarSection.course)
    var grammarSections: [GrammarSection] = []
    
    @Relationship(deleteRule: .cascade, inverse: \StudyPlan.course)
    var studyPlan: StudyPlan?
    
    init(id: String, name: String, foreignLanguage: String) {
        self.id = id
        self.name = name
        self.foreignLanguage = foreignLanguage
    }
}

@Model
final class Word {
    @Attribute(.unique) var id: String
    var english: String
    var foreign: String
    var dayIntroduced: Int
    var difficulty: Int = 1
    
    @Relationship(inverse: \Course.words)
    var course: Course?
    
    func isInRevisionD1(currentDay: Int) -> Bool { dayIntroduced == currentDay - 1 }
    func isInRevisionD3(currentDay: Int) -> Bool { 
        dayIntroduced >= currentDay - 3 && dayIntroduced < currentDay 
    }
}

@Model
final class Phrase {
    @Attribute(.unique) var id: String
    var english: String
    var foreign: String
    var dayIntroduced: Int
    var difficulty: Int = 1
    
    @Relationship(inverse: \Course.phrases)
    var course: Course?
    
    func isInRevisionD1(currentDay: Int) -> Bool { dayIntroduced == currentDay - 1 }
    func isInRevisionD3(currentDay: Int) -> Bool { 
        dayIntroduced >= currentDay - 3 && dayIntroduced < currentDay 
    }
}

@Model
final class GrammarSection {
    var number: Int
    var title: String
    var content: String                     // Full markdown or plain text
    
    @Relationship(inverse: \Course.grammarSections)
    var course: Course?
}

// MARK: - Study Plan
@Model
final class StudyPlan {
    @Relationship(inverse: \Course.studyPlan)
    var course: Course?
    
    var weekPlans: [WeekPlan] = []
}

@Model
final class WeekPlan {
    var weekNumber: Int
    var dayPlans: [DayPlan] = []
}

@Model
final class DayPlan {
    var dayNumber: Int
    var morningActivities: [ActivityDefinition] = []
    var eveningActivities: [ActivityDefinition] = []
}

@Model
final class ActivityDefinition {
    @Attribute(.unique) var id: String
    var type: ActivityType
    var metadata: [String: String] = [:]
    var order: Int = 0
}

// MARK: - Progress
@Model
final class UserActivityCompletion {
    var date: Date                      // normalized to midnight
    var activityId: String
    var isCompleted: Bool = false
    var completedAt: Date?
    
    @Relationship(inverse: \UserProfile.completions)
    var user: UserProfile?
}

// MARK: - Helper Extensions
extension Date {
    static func startOfDay(_ date: Date = Date()) -> Date {
        Calendar.current.startOfDay(for: date)
    }
}
```

## 4.3 Required Supporting Services
- `ContentSeeder` service (loads bundled JSON + Markdown into models).
- `StudyPlanEngine` (builds daily sessions, D-1/D-3 revisions in original order).
- `ProgressRepository` (all completion logic).

**Cursor Instructions**: Implement **exactly** as shown. Add indexes on `dayIntroduced`, `date`, etc. Create basic `SchemaMigrationPlan`.

This file is the single source of truth for all data structures.
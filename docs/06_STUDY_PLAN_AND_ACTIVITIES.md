# 06 STUDY PLAN & ACTIVITIES

## Study Plan Structure
- Weekly cycles (7 days)
- Each day has **Morning** and **Evening** sessions
- Sessions are arrays of Activity definitions

## Activity Types (All must be implemented)

**Words & Phrases** (identical structure):
- New [Words/Phrases] – Two-column list (English → Foreign)
- New [Words/Phrases] – Flashcards E→F
- New [Words/Phrases] – Flashcards F→E
- Revision [Words/Phrases] D-1 (previous day only)
- Revision [Words/Phrases] D-3 (all words from last 3 days in original order)

**Grammar**
- Grammar activity for section K shows the GrammarSection where number == K
- Next grammar activity automatically advances the number.

All activities support "Mark as Done".

## Flashcard Rules
- Tap to flip or swipe
- D-3 revision must preserve original learning order (not randomized or by SRS)
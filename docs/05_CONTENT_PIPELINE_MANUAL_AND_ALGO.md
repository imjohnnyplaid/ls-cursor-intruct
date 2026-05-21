# 05 CONTENT PIPELINE – Manual + Algorithmic Layer

## MVP: Manual Content (Small Team)
- Bundled JSON + Markdown in app bundle.
- Admin import via FilePicker → parse → SwiftData.
- Words/Phrases have `dayIntroduced`, `proficiencyTags`, `difficulty`.

## Future Algorithmic Layer (Phase 2+)
- Use local proficiency tracking (correct/incorrect streaks per word).
- Source: Bundled frequency lists + admin-curated expansions (open-source word lists like Wiktionary dumps).
- No external API in MVP (privacy + cost). Algorithm runs locally on device using user history.
- Generate “next set” of 10-20 new + revision words, increasing difficulty.

For V1: Focus on manual + basic proficiency tagging. Algorithm stub ready for later.
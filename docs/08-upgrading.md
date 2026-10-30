# 08 — Upgrading from Free and Previous Versions

[← Documentation index](../README.md)

## A. Bring Free Starter Pack conversations into Full

**Conversation Data Tables are compatible.** You do not have to rewrite existing dialogue lines.

1. Back up your project.
2. Install the Full Framework from Fab.
3. Retain or transfer your existing conversation Data Tables.
4. Add `AC_DialogueManager` to the appropriate player Blueprint.
5. Initialize it with `Dialogue Create Widgets`.
6. Connect the Data Tables to `Dialogue Play Conversation`.
7. Test conversation content, events, durations, and speaker names.
8. Enable Common UI if you plan to use decisions.

New features such as decisions and Rich Text require additional configuration; migrating conversation data does not automatically create branching choices.

## B. Upgrade older GameInstance-based integrations

Earlier documentation used `BP_DialogueGameInstance` and widget/playback calls from that GameInstance. The current architecture uses `AC_DialogueManager` on the player.

1. Create a project backup before importing the new version.
2. Identify old GameInstance-based calls and only remove or replace them after confirming their new equivalents.
3. Add `AC_DialogueManager` to the player and replace conversation initialization with `Dialogue Create Widgets` → `Dialogue Play Conversation`.
4. Keep existing Data Tables where row structures remain compatible.
5. Review widget references, audio interfaces, language handling, and events; test each feature.
6. If the project's old GameInstance has other responsibilities, do not remove it without checking those dependencies.

**Important:** This is not an automatic Blueprint migration. Conversation data can be reused, but project-level connections may need to be rewired.

## C. Migration checklist

- The first conversation displays the expected text.
- Events reach receivers implementing `BPI_DialogueEvents`.
- Audio and localization work.
- The appropriate layout appears.
- Decision and Rich Text features work after separate configuration in Full.

Next: [Troubleshooting](09-troubleshooting.md).

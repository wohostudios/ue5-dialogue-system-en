# 10 — Architecture and Extending the Framework

[← Documentation index](../README.md)

## Confirmed Full architecture

```text
Player Character Blueprint
├── AC_DialogueManager
│   ├── ST_DialogueLine / conversation Data Tables
│   ├── Text, language, audio, timing and events
│   └── Dialogue widgets (subtitles / centered)
├── AC_DecisionManager
│   ├── ST_DecisionLine / decision Data Tables
│   └── Common UI and UIRoot Ref
└── DialogueVoice (Audio Component in the demo)
```

![Player components](../images/instalacion-actor-component.png)

## Decision flow

1. `Decisions Create Widgets` initializes the root UI.
2. `Decision Play Choices` loads the choices from a Data Table.
3. `BPI_PlayerDialogue` returns `Selected Result ID` to the requester.
4. Game logic handles that ID by changing language, performing an action, or starting another conversation.

## Dialogue flow

1. `Dialogue Create Widgets` prepares the UI.
2. `Dialogue Play Conversation` takes a Data Table and `Current Language`.
3. The manager reads `ST_DialogueLine`, selects localized text/audio/prompts, and updates the widget.
4. It plays voice audio, manages timing, and dispatches gameplay events.

## Confirmed Blueprint Interfaces

- `BPI_DialogueEvents`: `ExecuteDialogueEvent` and conversation continuation.
- `BPI_PlayerDialogue`: `ExecuteDialogueFromDecision`, returning `Selected Result ID`, `Requester`, and `Active Decision Widget`.



**`BP_DialogueGameInstance` is not required.**

Next: [Troubleshooting](09-troubleshooting.md).

# 09 — Troubleshooting and FAQ

[← Documentation index](../README.md)

### The dialogue does not appear

Check that `AC_DialogueManager` is attached to the right player, `Dialogue Create Widgets` runs before `Dialogue Play Conversation`, and the `Conversation` pin points to a valid Data Table.

### Decision buttons do not appear

Make sure **Common UI** is enabled; restart the editor if required. Check that the Full decision table and widgets are used. If buttons appear but cannot receive focus, inspect the widget's focus/navigation setup.

### Gameplay events do nothing

The receiver needs to implement `BPI_DialogueEvents`. Confirm `TriggerEvent`, `Event Name`, and the corresponding `Switch on Name` branch. Remember that several actors with that interface may receive the same event.

### Audio never finishes or the next line does not appear

Check `Audio_ES` / `Audio_EN`, `Duration`, `DelayAfter`, and the component's audio-completion logic. Verify whether older interface documentation matches the current Full package.

### Colored words are not displayed

Verify that the widget uses a Rich Text Block, with the correct style table and markup. Check the Data Table for exact style names.

### The wrong language is displayed

Verify `Current Language`, and confirm content exists in the corresponding `Text_ES`, `Text_EN`, `Audio_ES`, `Audio_EN`, and `PromptText` fields.

### Must I remake conversations from Free?

No. Reuse the existing conversation Data Tables and connect them to Full. Configure decisions and Rich Text separately.

### Is `BP_DialogueGameInstance` required?

No. Current Full uses the `AC_DialogueManager` and `AC_DecisionManager` Actor Components.

## Need help?

Share screenshots of the nodes, the exact asset name, your Unreal Engine version, and the error message. [Spanish-speaking Discord community](https://discord.gg/JY5JTQaWqn).

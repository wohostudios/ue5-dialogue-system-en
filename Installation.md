# Installation Guide

This guide explains how to install and configure **Dialogue System** in an Unreal Engine 5 project.

---

# Requirements

Before getting started, make sure your project meets the following requirements:

- Unreal Engine 5
- Blueprint-based project
- Configured Audio System
- Enhanced Input (Optional)

---

# Step 1. Migrate the Content

Migrate the **DialogueSystem** folder into your project or install it from Fab.

It is recommended to keep the original folder structure to avoid broken references.

---

# Step 2. Configure the GameInstance

Go to:

```text
Project Settings

↓

Maps & Modes

↓

Game Instance Class
```

Select:

```text
BP_DialogueGameInstance
```

From this point on, the entire framework will be managed by the GameInstance.

---

# Step 3. Create the Widgets

Dialogue System uses two primary widgets.

```text
WBP_Subtitles
```

Displays subtitles at the bottom of the screen.

---

```text
WBP_CenterDialogue
```

Displays centered dialogue messages.

---

Both widgets implement:

```text
BPI_DialogueWidget
```

They should not be removed.

---

# Step 4. Create the Widgets at Game Startup

In the **BeginPlay** event of the Level Blueprint (or any other initialization point), call:

```text
Dialogue_CreateWidgets()
```

This function creates both widgets and keeps them in memory for the duration of the conversation.

---

# Step 5. Configure the Audio System

The Blueprint responsible for playing dialogue audio (usually the Character) must implement:

```text
BPI_DialogueAudio
```

Implement the function:

```text
PlayDialogueAudio()
```

When the audio finishes playing, it must call:

```text
AudioFinished()
```

through:

```text
BPI_DialogueAudioFinished
```

This allows the framework to know when it can continue to the next dialogue line.

---

# Step 6. Configure Trigger Events

Any Blueprint that should respond to dialogue events must implement:

```text
BPI_DialogueEvents
```

Implement:

```text
ExecuteDialogueEvent(EventName)
```

The **EventName** parameter contains the name of the event defined in the Data Table.

Example:

```text
OpenDoor

PlayMusic

SpawnEnemy
```

---

# Step 7. Configure Conversation Events

If your game needs to react when a conversation starts or ends, implement:

```text
BPI_DialogueConversation
```

Functions:

```text
ConversationStarted()

ConversationFinished()
```

Common use cases:

- Disable player movement.
- Show cinematic black bars.
- Hide the HUD.
- Play background music.
- Start a cinematic.

---

# Step 8. Configure Localization

Dialogue System supports multiple languages.

Currently included:

```text
ES
EN
```

Each dialogue line must contain:

```text
Text_ES
Text_EN

Audio_ES
Audio_EN

PromptText_ES
PromptText_EN
```

The active language is automatically selected by the GameInstance using:

```text
CurrentLanguage
```

---

# Step 9. Create a Data Table

Create a Data Table using the following structure:

```text
ST_DialogueLine
```

Each row represents a single dialogue line.

---

# Step 10. Fill in the Dialogue Data

Each row allows you to configure the following fields.

## Speaker

The name of the speaking character.

---

## Text

Localized dialogue text.

---

## Audio

Localized dialogue audio.

---

## DisplayType

The widget style used to display the dialogue.

Available values:

```text
Subtitle

Center
```

---

## Type

Dialogue line type.

Available values:

```text
Dialogue

Input

Event

Cinematic
```

---

## ShowSpeaker

Determines whether the speaker's name should be displayed.

```text
true

false
```

---

## PromptText

Contextual text displayed below the dialogue.

Example:

```text
Press E to Continue.
```

If left empty, the widget will remain hidden.

---

## SubtitleColor

The color used for the subtitle text.

---

## AllowSkip

Allows the player to skip the dialogue line.

```text
true

false
```

---

## DelayAfter

The amount of time to wait before displaying the next dialogue line.

---

## TriggerEvent

The name of the custom event to execute.

Example:

```text
OpenDoor

PlayMusic

SpawnEnemy
```

---

# Step 11. Start a Conversation

From any Blueprint, get the GameInstance.

Call:

```text
PlayConversation()
```

Parameters:

- Data Table

The framework will automatically handle the entire conversation.

---

# Conversation Flow

```text
PlayConversation()

↓

ConversationStarted()

↓

CreateWidgets()

↓

Load Conversation

↓

Initialize Variables

↓

Select Language

↓

Select Widget

↓

Update Widget

↓

Play Audio

↓

Wait

↓

TriggerEvent

↓

Clear Dialogue

↓

DelayAfter

↓

Next Dialogue

↓

ConversationFinished()

↓

Hide Widgets
```

---

# Adding a New Widget

To create a new dialogue display style:

1. Create a Widget.

2. Implement:

```text
BPI_DialogueWidget
```

3. Add a new value to the following enum:

```text
EDialogueDisplay
```

4. Update:

```text
Dialogue_UpdateActiveWidget()
```

No other part of the framework needs to be modified.

---

# Adding a New Language

Add the new fields to the dialogue structure.

Example:

```text
Text_FR

Audio_FR

PromptText_FR
```

Update the language selection inside the GameInstance.

No changes to the widgets are required.

---

# Adding a New Trigger Event

Enter the event name in the Data Table.

Example:

```text
OpenDoor
```

Implement the corresponding logic inside:

```text
ExecuteDialogueEvent()
```

No modifications to the framework are required.

---

# Troubleshooting

## The dialogue interface does not appear

Verify that:

- The GameInstance is configured.
- The widgets have been created.
- `Dialogue_CreateWidgets()` has been executed.

---

## The dialogue never continues

Verify that the Blueprint responsible for audio playback calls:

```text
AudioFinished()
```

---

## TriggerEvent does not work

Verify that the Blueprint implements:

```text
BPI_DialogueEvents
```

---

## PromptText does not appear

Verify that:

- The text is not empty.
- The widget implements `UpdateDialogue()`.
- `PromptText` is being sent from the GameInstance.

---

## The widget does not change

Verify:

```text
DisplayType
```

and

```text
Dialogue_UpdateActiveWidget()
```

---

# Recommendations

- Keep all dialogue logic inside the GameInstance.
- Avoid adding game-specific logic to the framework.
- Use Blueprint Interfaces for all communication.
- Keep conversations completely Data-Driven.
- Always implement `BPI_DialogueWidget` when creating new widgets.
- Use Trigger Events to connect dialogue with your game logic.

---

# Installation Checklist

Before using Dialogue System, verify that:

- [ ] BP_GameInstance is configured.
- [ ] `Dialogue_CreateWidgets()` has been executed.
- [ ] The Character implements `BPI_DialogueAudio`.
- [ ] The Character implements `BPI_DialogueAudioFinished`.
- [ ] The widgets implement `BPI_DialogueWidget`.
- [ ] The receiving Blueprint implements `BPI_DialogueEvents`.
- [ ] A Data Table has been created using `ST_DialogueLine`.
- [ ] Languages have been configured.
- [ ] Trigger Events have been implemented.

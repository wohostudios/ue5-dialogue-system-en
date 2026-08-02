# Dialogue System

Dialogue System is a modular, decoupled, and Data Table-driven dialogue framework for Unreal Engine 5, designed to be reusable in any project developed entirely with Blueprints.

Its goal is to provide a reusable, decoupled, and data-driven dialogue system that can be easily integrated into any project without depending on game-specific logic.

---

# Features

- ✅ Data Table-driven conversations.
- ✅ Text and audio localization.
- ✅ Automatic synchronization with audio duration.
- ✅ Delay system between dialogue lines.
- ✅ Allows players to skip dialogue lines.
- ✅ Dialogue lines that require player interaction.
- ✅ Custom event execution.
- ✅ Multiple dialogue interface styles.
- ✅ Contextual messages (Prompt Text).
- ✅ Show or hide the speaker's name.
- ✅ Subtitle color customization.
- ✅ Fully decoupled architecture using Blueprint Interfaces.
- ✅ Conversation start and end events.

---

# Architecture

All dialogue logic is centralized in:

```text
BP_GameInstance
```

The GameInstance is responsible for:

- Managing the dialogue flow.
- Selecting the active language.
- Managing the active dialogue widget.
- Playing dialogue audio.
- Handling delays and player input.
- Executing events.
- Managing the conversation state.

---

# Interface-Based Architecture

The system uses Blueprint Interfaces to completely decouple its components.

It currently includes the following interfaces:

## BPI_DialogueAudio

Responsible for playing dialogue audio.

Functions:

```text
PlayDialogueAudio()
```

---

## BPI_DialogueAudioFinished

Notifies the system when dialogue audio has finished.

Functions:

```text
AudioFinished()
```

---

## BPI_DialogueWidget

Implemented by every dialogue widget.

Functions:

```text
UpdateDialogue()

ClearDialogue()

ShowWidget()

HideWidget()
```

---

## BPI_DialogueEvents

Allows dialogue lines to execute game events.

Functions:

```text
ExecuteDialogueEvent(EventName)
```

---

## BPI_DialogueConversation

Notifies when a conversation starts and ends.

Functions:

```text
ConversationStarted()

ConversationFinished()
```

---

# Data-Driven System

All dialogue information is stored in a Data Table.

Each row completely defines the behavior of a dialogue line.

The main properties are:

| Property | Description |
|-----------|-------------|
| Type | Dialogue line type |
| Speaker | Character speaking |
| Text | Localized text |
| Audio | Localized audio |
| DisplayType | Dialogue display type |
| PromptText | Contextual message |
| SubtitleColor | Subtitle color |
| AllowSkip | Allows the line to be skipped |
| DelayAfter | Delay before the next line |
| TriggerEvent | Custom event |

---

# Localization

The system currently supports:

- Spanish
- English

Each dialogue line contains:

```text
Text_ES
Text_EN

Audio_ES
Audio_EN

PromptText_ES
PromptText_EN
```

The active language is automatically selected through:

```text
CurrentLanguage
```

---

# Dialogue Types

The following dialogue types are currently available:

```text
Dialogue
Input
Event
Cinematic
```

Each type manages its own behavior through:

```text
Switch on EDialogueType
```

---

# Display Types

The framework currently provides two visual presentation styles:

```text
Subtitle
```

Displays dialogue as subtitles at the bottom of the screen.

```text
Center
```

Displays dialogue as a centered message.

The framework automatically activates the corresponding widget.

---

# Prompt Text

Each dialogue line can display an additional contextual message.

Examples:

```text
Press Q to Skip

Press E to Continue

Hold Shift to Run
```

If `PromptText` is empty, the widget automatically hides it.

---

# Audio System

The framework supports:

- Localized audio.
- Dialogue lines without audio.
- Automatic waiting until audio playback finishes.
- Synchronization between text and voice.

The conversation only continues when:

```text
Audio Finished

AND

Duration Finished
```

---

# Delay After

Each dialogue line can define a delay before displaying the next one.

Flow:

```text
Display Dialogue

↓

Clear Widget

↓

DelayAfter

↓

Display Next Dialogue
```

This allows natural pauses between dialogue lines.

---

# Skip System

Each dialogue line controls whether it can be skipped.

```text
AllowSkip = true
```

Allows the player to skip the current dialogue line immediately.

```text
AllowSkip = false
```

Ignores the player's skip input.

---

# Show Speaker

Each dialogue line determines whether the speaker's name should be displayed.

Controlled by:

```text
ShowSpeaker
```

---

# Subtitle Color

Each dialogue line can define its subtitle color using:

```text
EDialogueSubtitleColor
```

Examples:

```text
White
Gray
Yellow
Blue
Red
Green
```

---

# Trigger Events

A dialogue line can execute a custom game event.

Example:

```text
TriggerEvent

↓

OpenDoor
```

The framework sends:

```text
ExecuteDialogueEvent()
```

through a Blueprint Interface.

The receiving Blueprint decides which action to perform.

The dialogue framework never contains game-specific logic.

---

# Conversation Start and End

The framework exposes two global events:

```text
ConversationStarted()
```

```text
ConversationFinished()
```

These allow the game to react whenever a conversation starts or ends.

Common use cases include:

- Locking player movement.
- Showing or hiding the HUD.
- Starting background music.
- Saving progress.
- Launching cinematics.

---

# System Flow

```text
PlayConversation()

↓

ConversationStarted()

↓

Create Widgets

↓

Load Conversation

↓

Read Current Line

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

Check Continue

↓

Execute TriggerEvent

↓

Clear Widget

↓

DelayAfter

↓

Next Dialogue Line

↓

...

↓

ConversationFinished()

↓

Hide Widgets

↓

Reset Variables
```

---

# Design Principles

The framework was designed around five core principles.

## 1. Data-Driven

All dialogue data is defined through Data Tables.

No conversations are hardcoded inside Blueprints.

---

## 2. Decoupled

Game logic never depends on the internal implementation of the dialogue system.

Likewise, the dialogue framework never depends on game-specific logic.

All communication is performed through Blueprint Interfaces.

---

## 3. Modular

Every system component is independent.

- Audio
- UI
- Events
- Localization
- Dialogue Flow

---

## 4. Reusable

The framework can be easily integrated into different Unreal Engine projects.

---

## 5. Scalable

Adding new widgets, languages, events, or features does not require modifying the core framework.

---

# Current Version

## Dialogue System v1.0

Currently implemented:

- ✅ Complete dialogue flow.
- ✅ Localization.
- ✅ Audio synchronization.
- ✅ Multiple dialogue interface styles.
- ✅ Prompt Text.
- ✅ Trigger Events.
- ✅ Conversation lifecycle events.
- ✅ Skip system.
- ✅ Delay After.
- ✅ Subtitle color customization.
- ✅ Show/Hide Speaker.
- ✅ Blueprint Interface-based architecture.
- ✅ Fully Data-Driven framework.

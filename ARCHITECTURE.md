# Dialogue System Architecture

This document describes the internal architecture of **Dialogue System** and the design decisions made during its development.

Its purpose is to simplify framework maintenance and serve as a guide for future extensions.

---

# Philosophy

Dialogue System was designed around four fundamental principles.

## 1. Single Responsibility

Every component in the framework should have one and only one responsibility.

Examples:

- The GameInstance manages the dialogue flow.
- Widgets are responsible only for displaying information.
- The Character plays dialogue audio.
- Game events are executed through Blueprint Interfaces.

---

## 2. Data-Driven

All conversation data is stored in Data Tables.

The behavior of a conversation should never be hardcoded inside Blueprints.

Conversations only describe data.

They never contain gameplay logic.

---

## 3. Decoupling

No system knows the internal implementation of another.

All communication is performed through Blueprint Interfaces.

This allows components to be replaced without affecting the rest of the framework.

---

## 4. Reusability

The main goal of the project is to make the framework reusable in any Unreal Engine game.

For this reason, the framework never contains game-specific logic.

---

# Overall Architecture

```text
                    Data Table
                        │
                        ▼
                 BP_GameInstance
                        │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
     Widgets          Audio         Trigger Events
        │                │                │
        ▼                ▼                ▼
   Interfaces      Interfaces      Interfaces
```

BP_GameInstance acts as the core of the framework.

Every subsystem receives its instructions from this Blueprint.

---

# Component Responsibilities

## BP_GameInstance

The core of the framework.

Responsibilities:

- Start conversations.
- End conversations.
- Control the dialogue flow.
- Select the active language.
- Select the active widget.
- Manage dialogue timing.
- Handle skip requests.
- Execute Trigger Events.
- Maintain the current framework state.

BP_GameInstance never contains game-specific logic.

---

## Widgets

Widgets are responsible only for displaying information.

They never make decisions.

They never control the dialogue flow.

They never execute game events.

Their sole responsibility is to visually represent the data they receive.

Currently available:

```text
WBP_Subtitles

WBP_CenterDialogue
```

Both implement exactly the same Blueprint Interface.

---

## Character

The Character is currently responsible only for playing dialogue audio.

It does not control conversations.

It does not know about Data Tables.

It does not know about Widgets.

It does not know about Trigger Events.

---

# Blueprint Interfaces

All communication between systems is handled through Blueprint Interfaces.

The framework currently includes five interfaces.

---

## BPI_DialogueAudio

Responsible for audio playback.

```text
PlayDialogueAudio()
```

---

## BPI_DialogueAudioFinished

Notifies the framework when audio playback has finished.

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

Executes gameplay events.

```text
ExecuteDialogueEvent(EventName)
```

---

## BPI_DialogueConversation

Notifies when a conversation starts and ends.

```text
ConversationStarted()

ConversationFinished()
```

---

# Internal Flow

The framework always follows the same execution flow.

```text
PlayConversation()

↓

ConversationStarted()

↓

Load Conversation

↓

Current Line

↓

Initialize Variables

↓

Select Language

↓

Select Display Type

↓

Update Widget

↓

Play Audio

↓

Wait

↓

Dialogue_CheckContinue()

↓

Trigger Event

↓

Clear Dialogue

↓

DelayAfter

↓

Next Dialogue

↓

ConversationFinished()
```

The entire framework revolves around this flow.

---

# State System

The framework distinguishes between several internal states.

Example:

```text
None

Playing

WaitingForInput
```

Each state determines how the framework should respond.

---

# Localization System

Language selection is handled entirely inside the GameInstance.

Widgets never know which language the game is using.

They simply receive already localized text.

---

# Audio System

The framework supports two behaviors.

## Dialogue Line with Audio

The framework waits simultaneously for:

- Duration
- AudioFinished

The conversation continues only after both conditions have been met.

---

## Dialogue Line without Audio

The framework waits only for the configured Duration.

---

# Widget System

Widgets are created only once when the conversation begins.

They are never destroyed while the conversation is active.

The framework simply changes their visibility.

```text
Visible

Hidden
```

The active widget is always stored in:

```text
Dialogue_CurrentWidget
```

The rest of the framework never needs to know which widget is currently displaying the dialogue.

---

# Trigger Events

Trigger Events allow dialogue lines to notify the game when something happens.

Example:

```text
OpenDoor

PlayMusic

SpawnEnemy
```

The framework only sends the event name.

The receiving Blueprint decides what action to perform.

This design keeps both systems completely decoupled.

---

# Prompt Text

PromptText is a contextual message.

It can be used for:

- Tutorials.
- Instructions.
- Interaction prompts.
- Player guidance.

PromptText is never considered part of the main dialogue.

---

# Display Type

DisplayType controls how a dialogue line is presented.

Currently available:

```text
Subtitle

Center
```

New display types can be added without modifying the framework flow.

---

# Adding a New Widget

1. Create the Widget.
2. Implement `BPI_DialogueWidget`.
3. Add a new value to the `DisplayType` enum.
4. Update `Dialogue_UpdateActiveWidget()`.

No other part of the framework needs to be modified.

---

# Adding a New Language

1. Add the new fields to the Data Table.

```text
Text_FR

Audio_FR

PromptText_FR
```

2. Add the language to the corresponding enum.

3. Update the language selection inside the GameInstance.

Widgets require no modifications.

---

# Adding a New Trigger Event

1. Enter the event name in the Data Table.

```text
TriggerEvent

OpenDoor
```

2. Implement the corresponding logic in the receiving Blueprint.

No changes to BP_GameInstance are required.

---

# Best Practices

✔ Keep all dialogue logic inside the GameInstance.

✔ Keep widgets free of gameplay logic.

✔ Use Blueprint Interfaces for all communication.

✔ Keep conversations fully Data-Driven.

✔ Avoid direct references between systems.

✔ Avoid game-specific logic inside the framework.

---

# Project Goal

Dialogue System is not intended to be a dialogue system for a single game.

Its goal is to become a reusable, modular, and scalable framework that can serve as the foundation for any Unreal Engine project.

---
updated: 23 Oct 2024
authors:
  - Ong Tsien Jin
  - Gowri Bhat
---

# Preference Service

This service handles the user preference across the webapp. It is responsible for persisting these states into local storage and listening to changes to local storage.

---

## @label(service) PreferenceService

### Attributes

#### @label(attr) $chatMode

`BehaviorSubject<ChatMode>` is the state controller for the chat mode, which manages whether the app operates in text or voice mode.

#### @label(attr) $language

`BehaviorSubject<Language>` controls the preferred language for the application.

### Methods

#### @label(private) @label(meth) Initialise Preferences

    private initialisePreferences(): void

Description
: Initializes the preferences by subscribing to `BehaviorSubjects` and syncing them with local storage.

#### @label(private) @label(meth) Listen To Local Storage Events

    private listenToLocalStorageEvents(): void

Description
: Listens for local storage changes and updates the corresponding preference states in real-time.

#### @label(private) @label(meth) Load From Local Storage

    private loadFromLocalStorage<T>(key: PreferenceKey, defaultValue: T): T

Description
: Loads a preference value from local storage, or uses the provided default value if no value is found in storage.

Parameters
: `key` (`PreferenceKey`): The key under which the preference is stored.
: `defaultValue` (`T`): The default value to use if the preference is not found in storage.

Returns
: `T`

#### @label(private) @label(meth) Set To Local Storage

    private setToLocalStorage<T>(key: string, value: T): void

Description
: Stores a preference value in local storage.

Parameters
: `key` (`string`): The storage key for the preference.
: `value` (`T`): The value to store in local storage.

#### @label(meth) Set Chat Mode

    setChatMode(mode: ChatMode): void

Description
: Sets the chat mode (either voice or text mode) for the application.

Parameters
: `mode` (`ChatMode`): The chat mode to set, either `ChatMode.Voice` or `ChatMode.Text`.

#### @label(meth) Set Language

    setLanguage(language: Language): void

Description
: Sets the preferred language for the application.

Parameters
: `language` (`Language`): The selected language preference.

---

### Older Implementations @label(deprecated)

!!! WARNING "Not Currently Used"

    The following are older features previously handled by this service, but they are no longer in use. They remain documented here for reference but are not active in the current version of the app.

#### Show Live Transcription

Toggles live transcription of the user’s voice input during interactions. This would show a live text transcription as the user speaks.

#### Voice Interrupt

Allows the user to enable or disable the ability to interrupt ongoing audio playback with a voice command.

#### Detect Voice Start

Controls whether the app would automatically detect when the user starts speaking to begin recording.

#### Detect Voice End

Controls the automatic detection of the end of the user’s voice input, used to stop recording after a period of silence.

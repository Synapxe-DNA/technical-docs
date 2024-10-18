---
updated: 18 Oct 2024
authors:
  - Ong Tsien Jin
---

# Conversation Broker Service

This service is responsible for managing interaction between UI components and other services related to chat (and voice)
functionality.

!!! WARNING "Isolation of business logic"

    All interaction with voice and chat MUST come through this service. Logic related to all global interaction has been
    encapsulated into this service, so that UI components can focus on UI states and only UI related logic that responds
    to changes reflected in this service.

    This service should reflect the global state of events necessary for UI components.

---

## @label(service) ConvoBrokerService

### Attributes

#### @label(private) @label(attr) recorder

`AudioRecorder` is the object responsible for recording user audio.

#### @label(private) @label(attr) activeProfile

`Profile | undefined` tracks the currently active profile. This is updated by a subscription instantiated by the constructor.

#### @label(attr) $micState

`BehaviorSubject<MicState>` is the global controller for the mic state, used to control the UI state of the main mic button
on the voice page.

#### @label(attr) $sendTimeout

`BehaviorSubject<boolean>` is a boolean flag used to signal if a timeout should be invoked.

#### @label(attr) $isWaitingForVoiceApi

`BehaviorSubject<boolean>` to track if a response has been made from the backend API regarding voice chat.

### Voice Chat Related Methods

#### @label(private) @label(meth) Initialise Voice Chat

    private async initVoiceChat(): Promise<void>

Description
: Method to initialise voice activity detection, and initialise `AudioRecorder`. It is called in the constructor of the service.
: This method instantiates the subscription for VAD detection, and contains side effects.

!!! NOTE "VAD is only active in voice mode"

    To prevent undesired VAD, the initialised subscribtion to handle VAD will only trigger side effects when the user is
    in `ChatMode.Voice`.

#### @label(private) @label(meth) Start Recording

    private handleStarteRecording(): void

Description
: Method to trigger the start of audio recording.

#### @label(private) @label(meth) Stop Recording

    private handleStopRecording(): void

Description
: Handle the stopping of recording, and post process the recorded audio. The recorded audio will be sent to the backend
as a voice message.

#### @label(private) @label(meth) Play Base64 Encoded Audio

    private async playAudioBase64(val:string): Promise<void>

Description
: Method to play a base 64 encoded audio. This induces side effects in `AudioPlayerService`.

Parameters
: `val` (`string`): Base 64 encoded audio file.

#### @label(private) @label(meth) Send Voice

    private async sendVoice(audio: Blob, profile: Profile): Promise<void>

Description
: Sends audio blob to the backend for LLM and voice chat functionality; it will handle the responses and updating of
states related to voice chat. Side effects include audio player and chat message service.

Parameters
: `audio` (`Blob`): Blob of audio recording file.
: `profile` (`Profile`): Profile used in the conversation

#### @label(meth) Mic Button Click

    handleMicButtonClick(): void

Description
: Method used in callbacks when the mic button to trigger audio recording actions has been clicked.

### Text Chat Related Methods

#### @label(meth) Send Chat

    async sendChat(message: string, profile: Profile): Promise<void>

Description
: Method to send a chat message. This will handle the persistence of messages into IndexedDB, and interacting with the
backend via the `EndpointService`.

Parameters
: `message` (`string`): User input message
: `profile` (`Profile`): Active profile used in the conversation.

### Feedback Related Methods

#### @label(meth) Send Feedback

    async sendFeedback(feedback: Feedback)

Description
: Method to send a feedback object. This will handle the persistence of messages into IndexedDB, and interacting with the
backend via the `EndpointService`.

Parameters
: `feedback` (`feedback`): Feedback object

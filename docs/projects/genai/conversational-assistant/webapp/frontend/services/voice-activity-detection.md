---
updated: 23 Oct 2024
authors:
  - Ong Tsien Jin
---

# Voice Activity Detection @label(deprecated)

This service is used to handle Voice Activity Detection (VAD).

The current implementation uses the browser's [`SpeechRecognition` API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition) to detect speech. Transcription is still handled by the backend due to the following reasons.

1. Multi language support. The browser requires the user to specify the language; leaving this capability to the backend allows us to automatically detect the user's language.
2. Limited browser support. Not all browsers support this API (e.g. Arc, Firefox), leaving this to our own API allows us to have full control and simplify the process of Speech to Text (STT) transcription.

!!! WARNING "Fallback required"

    A fall back VAD detector needs to be implemented in the event of an unsupported browser. This method can be done using live transcription from our backend, or using alternative methods (e.g. volume levels).

---

## @label(service) VadService

### Attributes

#### @label(private) endTimeout

`number` to keep track of timeout calls that are used in `VadService.start()` to emit an event when speech has ended.

#### @label(private) $speech

`Subject<void>` emits an event whenever speech has been detected; fired through a subscription in `VadService.configSpeechRecognition()` as a side effect.

#### @label(private) recognition

`SpeechRecognition` object from the browser.

### Methods

#### @label(private) @label(meth) Configure

    private configSpeechRecofnition(): void

Description
: Method to instantiate the speech recognition object. Called in the constructor.

#### @label(meth) Start

    start(): Observable<VoiceActivity>

Description
: Method to get an observable to keep track of VAD activity.

Returns
: `Observable<VoiceActivity>`

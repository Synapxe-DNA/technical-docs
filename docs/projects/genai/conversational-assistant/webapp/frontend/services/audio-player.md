---
updated: 23 Oct 2024
authors:
  - Ong Tsien Jin
---

# Audio Player Service

This service is responsible for playing all sounds on the frontend. This includes:

- LLM audio response (voice chat)
- Text to speech

---

## @label(service) AudioPlayerService

To facilitate audio streaming from LLM voice responses, it implements an audio queue system. Audio blobs are played in the sequence they are added in. This implementation was due to how audio files of the LLM response was received on the frontend.

!!! WARNING "Assumption"

    The current implementation generally assumes that there will only be one source of audio, voice LLM and TTS included.

### Attributes

#### @label(private) @label(attr) audioElement

`HTMLMediaElementWithCaptureStream` source of audio being played.

#### @label(private) @label(attr) queue

`Blob[]` contains audio files to be played.

#### @label(attr) $stream

`BehaviorSubject<MediaStream|null>` keeping track of the current audio stream. This will be consumed by the waveform visualiser.

#### @label(attr) $playing

`BehaviorSubject<boolean>` to keep track of the current playing state.

### Methods

#### @label(private) @label(meth) Play Next in Queue

    private playNextInQueue(): void

Description
: This method will play the next audio in the queue.

#### @label(meth) Get Audio Stream

    getAudioStream(): BehaviorSubject<MediaStream|null>

Description
: Public method to retrieve the `BehaviorSubject` to track the current audio stream source.

Returns
: `BehaviorSubject<MediaStream|null>`

#### @label(meth) Play

    play(...blob: Blob[]): void

Description
: Method to add an audio file to the queue to be played.

Parameters
: `...blob` (`...Blob[]`): Destructed array of `Blob`s to be played. Will be added to a queue.

#### @label(meth) Force Play

    forcePlayAndReplace(blob: Blob): void

Description
: This method is to clear the current queue of audio files, and play the provided `Blob` audio.

Parameters
: `blob` (`Blob`): Audio file to be played

#### @label(meth) Stop and Clear

    stopAndClear(): void

Description
: This method clears the current queue, stops the audio from being played and updates local states.

#### @label(meth) Play Start Voice Audio

    playStartVoiceAudio(): void

Description
: This method plays a pre-defined audio file (`startvoice.mp3`) from the app's assets, used to signal the start of voice interaction.

#### @label(meth) Play Stop Voice Audio

    playStopVoiceAudio(): void

Description
: This method plays a pre-defined audio file (`stopvoice.mp3`) from the app's assets, used to signal the end of voice interaction.

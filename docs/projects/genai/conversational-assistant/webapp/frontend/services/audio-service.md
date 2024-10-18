---
updated: 18 Oct 2024
authors:
  - Alexander Lee
---

# Audio Service

This service is responsible for managing the microphone resources on the frontend.

---

## @label(service) AudioService

This service requests for the microphone resource when necessary during voice input, and releases the microphone resource after voice recording has ended.

### Methods

#### @label(private) @label(meth) Get Mic Input

    async getMicInput(): Promise<MediaStream>

Description

: This method will retrieve the microphone resource from the device.

#### @label(async) @label(meth) Stop audio tracks

    async stopTracks(stream: MediaStream)

Description

: This method will release all audio tracks and release the microphone resource.

#### @label(async) @label(meth) Stop audio tracks

    async mergeAudioStreams(...streams: MediaStream[]): Promise<MediaStream>

Description

: This method merges the audio streams into 1 audio stream.

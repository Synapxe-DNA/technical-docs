---
updated: 21 October 2024
authors: Lyndon Lim
---

# @label(type) TranscriptionResponse

TranscriptionResponse pydantic model contains parameters that will be returned to the frontend.

Used for `/ws/transcribe` endpoint

=== "Python"

    ```python
    class TranscriptionResponse(BaseModel):
        text: str
        is_final: bool

    ```

## Attributes

### @label(attr) text

`string` The transcribed text.

### @label(attr) is_final

`bool` The flag to indicate if the transcription is final or not.

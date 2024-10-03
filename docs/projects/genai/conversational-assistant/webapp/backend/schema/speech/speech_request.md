---
updated: 1 October 2024
authors: Lyndon Lim
---

# @label(type) SpeechRequest

SpeechRequest pydantic model contains required parameter to be sent to Azure Speech Service for text to speech conversion.

Used for `/speech` endpoint

=== "Python"

    ```python
    class SpeechRequest(BaseModel):
        text: str
    ```

## Attributes

### @label(attr) text

`string` The text to be converted to speech.

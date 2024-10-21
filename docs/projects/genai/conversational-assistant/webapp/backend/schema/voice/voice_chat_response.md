---
updated: 1 October 2024
authors: Lyndon Lim
---

# @label(type) VoiceChatResponse

VoiceChatResponse pydantic model contains parameters that will be returned to the frontend.
It does not contain any text chunk as the frontend will not be displaying it.

Used for `/voice/stream` endpoint.

=== "Python"

    ```python
    class VoiceChatResponse(BaseModel):
        response_message: str
        audio_base64: str
        sources: List[Source]
    ```

## Attributes

### @label(attr) response_message

`string` The response message from the LLM.

### @label(attr) audio_base64

`string` The audio response from the LLM in base64 format.

### @label(attr) sources

`List[Source]` A list of sources without text chunk that the LLM has used to generate the response.

---
updated: 30 Septemeber 2024
authors: Lyndon Lim
---

# @label(type) TextChatResponseWithChunk

TextChatResponseWithChunk pydantic model contains parameters that will be returned to the frontend.

Used for `/chat` endpoint.

=== "Python"

    ```python
    class TextChatResponseWithChunk(BaseModel):
        response_message: str
        sources: List[SourceWithChunk]
    ```

## Attributes

### @label(attr) response_message

`string` The response message from the LLM.

### @label(attr) sources

`List[SourceWithChunk]` A list of sources with text chunk that the LLM has used to generate the response.

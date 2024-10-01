---
updated: 30 Septemeber 2024
authors: Lyndon Lim
---

# @label(type) TextChatResponse

TextChatResponse datatype contains parameters that will be returned to the frontend.
It does not contain any text chunk as the frontend will not be displaying it.

Used for `/chat/stream` endpoint.

=== "Python"

    ```python
    class TextChatResponse(BaseModel):
        response_message: str
        sources: List[Source]
    ```

## Attributes

### @label(attr) response_message

`string` The response message from the LLM.

### @label(attr) sources

`List[Source]` A list of sources without text chunk that the LLM has used to generate the response.

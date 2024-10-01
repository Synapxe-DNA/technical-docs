---
updated: 1 October 2024
authors: Lyndon Lim
---

# @label(type) ChatMessageWithSource

ChatMessageWithSource pydantic model contains parameters of a message that conforms to OpenAPI `ChatCompletionMessageParam` schema.
It contain sources.

=== "Python"

    ```python
    class ChatMessageWithSource(BaseModel):
        role: str
        content: str
        sources: List[SourceWithChunk] | List[Source]
    ```

## Attributes

### @label(attr) role

`string` The role of the chat message. It can be either 'user' or 'assistant'.

### @label(attr) content

`string` The content of the chat message.

### @label(attr) sources

`List[SourceWithChunk]` or `List[Source]` A list of sources with or without text chunk that the LLM has used to generate the response.

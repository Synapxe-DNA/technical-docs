---
updated: 1 October 2024
authors: Lyndon Lim
---

# @label(type) ChatMessage

ChatMessage pydantic model contains parameters of a message that conforms to OpenAPI `ChatCompletionMessageParam` schema
=== "Python"

    ```python
    class ChatMessage(BaseModel):
        role: str
        content: str
    ```

## Attributes

### @label(attr) role

`string` The role of the chat message. It can be either 'user' or 'assistant'.

### @label(attr) content

`string` The content of the chat message.

---
updated: 30 Septemeber 2024
authors: Lyndon Lim
---

# @label(type) TextChatRequest

TextChatRequest datatype contains required parameters for chat request.

Used for both `/chat/stream` and `/chat` endpoint.

=== "Python"

    ```python
    class TextChatRequest(BaseModel):
        chat_history: List[ChatMessage]
        profile: Profile
        query: ChatMessage
        language: str
    ```

=== "Actual Implementation"

    The actual implementation of this class inheirits `Request`.

    ```python
    class TextChatRequest(Request):
        pass
    ```

## Attributes

### @label(attr) chat_history

`List[ChatMessage]` A list of chat messages that have been exchanged between the user and the chatbot.

### @label(attr) profile

`Profile` The user profile.

### @label(attr) query

`ChatMessage` The user query.

### @label(attr) language

`string` The response language chosen by user

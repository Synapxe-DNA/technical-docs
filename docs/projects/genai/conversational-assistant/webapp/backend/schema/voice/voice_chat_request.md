---
updated: 1 October 2024
authors: Lyndon Lim
---

# @label(type) VoiceChatRequest

VoiceChatRequest pydantic model contains required parameters to be sent to the LLM for generating a response.

Used for `/voice/stream` endpoint.

=== "Python"

    ```python
    class VoiceChatRequest(BaseModel):
        chat_history: Optional[List[ChatMessage]] = []
        profile: Profile
        query: ChatMessage
        language: str
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

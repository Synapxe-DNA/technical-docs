---
updated: 15 October 2024
authors: Lyndon Lim
---

# @label(type) ChatHistory

ChatHistory pydantic model contains parameters of a chat history message that will be uploaded to CosmosDB
=== "Python"

    ```python
    class ChatHistory(BaseModel):
        id: str
        session_id: str
        created_at: str
        last_modified: str
        chat_messages: List[ChatMessageWithSource]
    ```

## Attributes

### @label(attr) id

`string` The unique identifier of the chat history. (Uses the same id as the session id)

### @label(attr) session_id

`string` The session id of the chat history.

### @label(attr) created_at

`string` The timestamp when the chat history was first created.

### @label(attr) last_modified

`string` The timestamp when the chat history was last modified.

### @label(attr) chat_messages

`List[ChatMessageWithSource]` A list of chat messages that have been exchanged between the user and the chatbot. Each chat message contains the role, content, and sources of the message.

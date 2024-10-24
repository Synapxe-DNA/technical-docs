---
updated: 1 October 2024
authors: Lyndon Lim
---

# @label(type) FeedbackRequest

FeedbackRequest pydantic model contains required parameters to be sent to CosmosDB for storing feedback.

Used for `/feedback` endpoint.

=== "Python"

    ```python
    class FeedbackRequest(BaseModel):
        date_time: str
        feedback_type: Literal["positive", "negative"]
        feedback_category: List[str]
        feedback_remarks: Optional[str] = ""
        user_profile: Profile
        chat_history: List[ChatMessageWithSource]
    ```

## Attributes

### @label(attr) date_time

`string` The date and time when the feedback was submitted.

### @label(attr) feedback_type

`Literal["positive", "negative"]` The type of feedback submitted.

### @label(attr) feedback_category

`List[str]` A list of feedback categories. E.g ["Outdated sources", "Not factually correct"]

### @label(attr) feedback_remarks

`Optional[str]` Optional remarks for the feedback.

### @label(attr) user_profile

`Profile` The user profile.

### @label(attr) chat_history

`List[ChatMessageWithSource]` A list of chat messages with sources that have been exchanged between the user and the chatbot.

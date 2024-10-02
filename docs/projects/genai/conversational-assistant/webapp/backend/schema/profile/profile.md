---
updated: 1 October 2024
authors: Lyndon Lim
---

# @label(type) Profile

Profile pydantic model contains parameters of a user profile.
=== "Python"

    ```python
    class Profile(BaseModel):
        profile_type: str
        user_age: Optional[int] = -1
        user_gender: Optional[str] = "male"
        user_condition: Optional[str] = ""
    ```

## Attributes

### @label(attr) profile_type

`string` The type of the profile. It can be either 'general' or 'myself' or "others".

### @label(attr) user_age

`int` The age of the user.

### @label(attr) user_gender

`string` Gender of the user. It can either be 'male' or 'female'

### @label(attr) user_condition

`string` The condition(s) of the user. The conditions are concatenated with a comma. For example, "diabetes, hypertension".

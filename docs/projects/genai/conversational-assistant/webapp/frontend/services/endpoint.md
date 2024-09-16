---
updated: 11 Sep 2024
authors:
  - Ong Tsien Jin
  - John-David Tan
---

# Endpoint Service

This service is used to interact with the backend. Understandably, this is the most crucial part in the context of
backend frontend integration. Given that (at the point of writing) the endpoints were still evolving, this service served
as a abstraction layer, so that the rest of the webapp will not break -- when API endpoints are updated, technically, this is the
only service that needs to be updated, _technically_.

---

## @label(service) EndpointService

### Utility Methods

#### @label(private) @label(meth) Message to Api Chat History

    private messageToApiChatHistory(message: Message[]): ApiChatHistory[]

Description
: Method to convert `Message` array into `ApiChatHistory` format for backend consumption.

Parameters
: `message` (`Message[]`): Array of messages to convert.

Returns
: `ApiChatHistory[]`

#### @label(private) @label(meth) Profile to Api Profile

    private profileToApiProfile(profile: Profile): ApiProfile

Description
: Method to convert `Profile` into `ApiProfile` format for backend consumption.

Parameters
: `profile` (`Profile`)

Returns
: `ApiProfile`

### Methods

#### @label(meth) Send Voice

    async sendVoice(
        query: string,
        profile: Profile,
        history: Message[]
    ): Promise<BehaviorSubject<VoiceResponse|null>>

Description
: Method to send query to the backend for a audio response.

Parameters
: `query` (`string`): String of user query.
: `profile` (`Profile`): Active profile used in the conversation.
: `history` (`Message[]`): Chat history in the conversation.

Returns
: `Promise<BehaviorSubject<VoiceResponse|null>>`

#### @label(meth) Send Chat

    async sendChat(
        message: Message,
        profile: Profile,
        history: Message[]
    ): Promise<BehaviorSubject<ChatReponse|null>>

Description
: Method to send chat (text) message to the backend for processing

Parameters
: `message` (`Message`): Chat message from user.
: `profile` (`Profile`): Active profile used in the conversation.
: `history` (`Message[]`): Chat history in the conversation.

Returns
: `Promise<BehaviorSubject<ChatResponse|null>>`

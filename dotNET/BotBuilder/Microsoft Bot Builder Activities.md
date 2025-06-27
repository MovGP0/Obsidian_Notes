A `Activity` represents a message sent to a channel. Typically serialized as JSON.

| Method        | Description                |
| ------------- | -------------------------- |
| `CreateReply` | create a response activity |

| Property       | Description                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------- |
| `conversation` | ID of the conversation                                                                            |
| `from`         | ID of the sender                                                                                  |
| `locale`       | locale information of the message                                                                 |
| `recipient`    | ID of the receiver                                                                                |
| `id`           | unique message ID of the request (GUID)                                                           |
| `replyToId`    | ID where to reply to                                                                              |
| `type`         | [ActivityType](#ActivityType) of the message                                                      |
| `text`         | Text of the message                                                                               |
| `textformat`   | Markdown message                                                                                  |
| `attachements` | files like images, audio, video, or documents                                                     |
| `timestamp`    | UTC timestamp of the message in ISO format                                                        |
| `serviceUrl`   | Originating URL where the message was sent from                                                   |
| `channelId`    | channel type the client uses to interacts with the bot (HTTP, Slack, SMS, E-Mail, Emulator, etc.) |

### ActivityType

| ActivityType            | Description                                                                            |
| ----------------------- | -------------------------------------------------------------------------------------- |
| `contactRelationUpdate` | User added or removed bot (Action property is `add` or `remove`)                       |
| `conversationUpdate`    | Other users have joined the conversation                                               |
| `deleteUserData`        | Delete user data and send confirmation message                                         |
| `message`               | Message to continue the conversation                                                   |
| `ping`                  | Test if URL is accessible. Return proper HTTP code when not authenticated or autorized |
| `typing`                | Indicate thet User/Bot is working on an reply                                          |

### Attachement

| Property       | Description                         |
| -------------- | ----------------------------------- |
| `content`      | (binary) Data of the attachement    |
| `contentType`  | MIME type of the attachement        |
| `contentUrl`   | URL where the attachement is found  |
| `name`         | Name of the attachement             |
| `thumbnailUrl` | URL to thumbnail of the attachement |

#### Special attachement types

| Attachement            | Description                                  |
| ---------------------- | -------------------------------------------- |
| `HeroCard`             | Large image, one or more buttons, and a text |
| `ThumbnailCard`        | Small image, one or more buttons, and a text |
| `Carousal`             | Multiple cards or images                     |
| `PromptDialog.Choice`  | Ask user to select one of multiple actions   |
| `PromptDialog.Confirm` | Ask user to confirm or cancel                |
| `PromptDialog.Text`    | Ask user for text input                      |

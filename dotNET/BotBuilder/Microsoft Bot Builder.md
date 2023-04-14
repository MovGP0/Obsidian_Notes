- [Microsoft.Bot.Builder Nuget Packages](https://www.nuget.org/packages?q=Microsoft.Bot.Builder)
- [Sources](https://github.com/microsoft/botbuilder-dotnet/tree/main/libraries)

## Important Classes
| Class/Interface    | Description                                             |
| ------------------ | ------------------------------------------------------- |
| `IBot`             | Base interface for bots                                 |
| `Activity`         | Message sent to the Bot                                 |
| `IConnectorClient` | Client interface to connect to a REST-based bot service |

## Activity

Message sent to or from the bot on a channel.
See: [[Microsoft Bot Builder Activities]]

## Channel

Channels are used to communicate with the bot or user. A bot might be called from and reply to multiple channel types.

## Conversation

A conversation is a series of messages between users and bots on an given channel.

| REST Method                                                                  | Description                                 |
| ---------------------------------------------------------------------------- | ------------------------------------------- |
| `POST https://domain/conversations`                                          | create a new conversation and return the ID |
| `POST https://domain/conversations/{converstationId}/activities`             | send an message to the conversation         |
| `GET https://domain/conversations/{converstationId}/activities/{activityId}` | retrive a specific message                  |

## IConnectorClient

Adapter to connect a bot with an channel.

```csharp
var connector = new ConnectorClient(new Uri(activity.ServiceUrl)); 
var length = (activity.Text ?? string.Empty).Length;
Activity reply = activity.CreateReply($"You sent {activity.Text} which was {length} characters");
await connector.Conversations.ReplyToActivityAsync(reply);
```

## See also

- [[Microsoft Bot Builder Forms]]
- [[Microsoft Bot Builder Dialogs]]
- [[Microsoft Bot Builder State Management]]

## Hints

- Use Skype API for handling calls
- Use Microsoft Cognitive Services for features like OCR, Text-to-Speech, Speech-to-Text, Sentiment Analysis, etc.


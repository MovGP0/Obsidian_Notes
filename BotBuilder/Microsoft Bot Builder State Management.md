NuGet [Microsoft.Bot.Connector](https://www.nuget.org/packages/Microsoft.Bot.Connector)

Data to store
- User data
- Conversation data
- Private conversation data

| REST Method                                                               | Description               |
| ------------------------------------------------------------------------- | ------------------------- |
| https://domain/botstate/{channelId}/users/{userId}                        | user data                 |
| https://domain/botstate/{channelId}/conversations/{conversationId}        | conversation data         |
| https://domain/botstate/{channelId}/conversations/{conversationId}/users/ | private conversation data |

`StateClient` class implements get, set, and delete methods.

## Retrive BotData

```csharp
var userId = activity.From.Id;
var channelId = activity.ChannelId;
var conversationId = activity.Conversation.Id;

var stateClient = activity.GetStateClient();
var botState = stateClient.BotState;
var userData = botState.GetUserData(channelId, userId);
var conversationData = botState.GetConversationData(channelId, conversationId);
var privateConversationData = botState.GetPrivateConversationData(channelId, conversationId, userId);
```

## Get/Set/Delete BotData

```csharp
var @value = userData.GetProperty<string>("propertyName");
userData.SetProperty<string>("propertyName", "value");
```

## Custom data store backend

Implement interface `Microsoft.Bot.Builder.IStorage` and register in dependency injection.

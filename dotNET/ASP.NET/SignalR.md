- uses HTTP 1.1
- Serializes data as JSON

## SignalR Hub

Startup
```csharp
services.AddSignalR();
```

```csharp
endpoints.MapHub<MyHub>("/my/hub");
```

Example
```protobuf
service ServiceName {
  rpc GetPerson(int32 personId) returns (Person);
  rpc SendPeople(stream Person) returns SuccessMessage;
  rpc GetPeople() returns (stream Person);
}
```

```csharp
using Microsoft.AspNetCore.SignalR;

public sealed class MyHub : Hub
{
    public async Task GetPersonAsync(
        int id,
        CancellationToken cancellationToken = default)
    {
        // ...
	    await Clients.Caller.SendAsync("GetPersonResponse", person, cancellationToken);
    }

    public async Task SendPeopleAsync(
        IAsyncEnumerable<Person> stream)
    {
        await foreach (var person in stream)
        {
             // ...
        }
        await Clients.Caller.SendAsync("SendPeopleResponse", "Success");
    }

    public async IAsyncEnumerable<Person> GetPeopleAsync(
        [EnumeratorCancellation] CancellationToken cancellationToken)
    {
        // ...
        foreach (var person in people)
        {
			if (cancellationToken.IsCancellationRequested) break;
	        yield return person;
        }
    }
}
```

## SignalR Client

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/microsoft-signalr/7.0.4/signalr.min.js" />
```

Create connection
```js
const connection = new singalR.HubConnectionBuilder()
	.withUrl("https://company.com/my/hub")
	.configureLogging(signalR.LogLevel.Information)
	.Build();
```

Send messages
```js
connection
    .invoke("GetPerson", id)
    .catch(err => { /* ... */ });
```

Receive messages
```js
connection.on("GetPersonResponse", message => {
    // ...
});
```

Send message stream
```js
var subject = new signalR.Subject();

for (let i = 0; i < people.length; i++)
{
    const person = people[i];
    subject.next(person);
}

subject.complete();
```

Receive message stream
```js
connection
     .stream("GetPeople")
     .subscribe({
         next: (person) => /* ... */
     });
```

Restart connection on error
```js
async function start(){
    try{
        await connection.start();
    }
    catch(error) {
        console.log(error);
        setTimeout(start, 500);
    }
}

connection.onclose(async() => {
    await start();
});

start();
```
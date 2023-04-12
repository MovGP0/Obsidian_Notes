Create a kernel with memory
```csharp
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Memory;

var memoryStore = new QdrantMemoryStore("QDRANT_ENDPOINT", qdrantPort, vectorSize: 1536, ConsoleLogger.Log);
var memoryStore = new VolatileMemoryStore();

var kernel = Kernel.Builder
    .Configure(c =>
    {
		c.AddOpenAIEmbeddingGeneration("ada", "text-embedding-ada-002", apiKey);
		c.AddOpenAITextCompletion("davinci", "text-davinci-003", apiKey, "");
    })
    .WithMemoryStorage(memoryStore)
    .Build();
```

Import the `TextMemorySkill` for the `recall` function
```csharp
using Microsoft.SemanticKernel.CoreSkills;

kernel.ImportSkill(new TextMemorySkill());
```

Add data to the memory
```csharp
await kernel.Memory.SaveInformationAsync("COLLECTION_NAME", id: "...", text: "...");
```

Search the memory
```csharp
var response = await kernel.Memory
	.SearchAsync(MemoryCollectionName, query)
	.FirstOrDefaultAsync();
```

Create custom prompt that uses the recall function
```csharp
const string skPrompt = @"
Information from previous conversations:
- {{$fact1}} {{recall $fact1}}
- {{$fact2}} {{recall $fact2}}
- {{$fact3}} {{recall $fact3}}
- {{$fact4}} {{recall $fact4}}
- {{$fact5}} {{recall $fact5}}

Chat:
{{$history}}

User: {{$userInput}}
ChatBot: ";
var chatFunction = kernel.CreateSemanticFunction(skPrompt, maxTokens: 200, temperature: 0.8);
```

Add information using a context
```csharp
var context = kernel.CreateNewContext();
context["fact1"] = "what is my name?";
context["fact2"] = "where do I live?";
context["fact3"] = "where is my family from?";
context["fact4"] = "where have I travelled?";
context["fact5"] = "what do I do for work?";
context[TextMemorySkill.CollectionParam] = MemoryCollectionName;
context[TextMemorySkill.RelevanceParam] = "0.8";
```

Invoke the chat
```csharp
var history = "";
context["history"] = history;

Func<string, Task> Chat = async (string input) =>
{
    // Save new message in the context variables
    context["userInput"] = input;

    // Process the user message and get an answer
    var answer = await chatFunction.InvokeAsync(context);

    // Append the new interaction to the chat history
    history += $"\nUser: {input}\nChatBot: {answer}\n";
    context["history"] = history;

    // Show the bot response
    Console.WriteLine("ChatBot: " + context);
};
```


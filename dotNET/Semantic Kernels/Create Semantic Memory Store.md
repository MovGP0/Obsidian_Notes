Create Semantic Memory Store
```csharp
var memoryStore = new QdrantMemoryStore("QDRANT_ENDPOINT", qdrantPort, vectorSize: 1536, ConsoleLogger.Log);
var memoryStore = new VolatileMemoryStore();
```

Register Semantic Memory Store
```csharp
Kernel.Builder
    // ...
	.WithMemoryStorage(memoryStore)
	.Build()
```

Remember data
```csharp
await kernel.Memory.SaveInformationAsync(
	collection: "COLLECTION_NAME",
	text: "TEXT",
	id: "INTERNAL_ID",
	description: "DESCRIPTION",
	cancellationToken);
```

Remember data with reference to source
```csharp
await kernel.Memory.SaveReferenceAsync(
	collection: "COLLECTION_NAME",  
    text: "TEXT",  
    externalId: "INTERNAL_ID"
    externalSourceName: "SOURCE",
    description: "DESCRIPTION",  
    cancellationToken 
);
```

Retrieve data by ID
```csharp
MemoryQueryResult? lookup = await kernel.Memory.GetAsync("COLLECTION_NAME", "INTERNAL_ID");
```

Search Data by vector similarity
```csharp
var searchResults = kernel.Memory.SearchAsync("COLLECTION_NAME", "some query input", limit: 3, minRelevanceScore: 0.8);

var i = 0;
await foreach (MemoryQueryResult item in searchResults)  
{
    Console.WriteLine($"Result {++i}:");
    Console.WriteLine("  URL:     : " + memory.Id);
    Console.WriteLine("  Title    : " + memory.Description);
    Console.WriteLine("  Relevance: " + memory.Relevance);
    Console.WriteLine();
}
```

Delete collection
```csharp
await memoryStore.DeleteCollectionAsync("COLLECTION_NAME");
```

List all collections
```csharp
var collections = await memoryStore.GetCollectionsAsync();
```


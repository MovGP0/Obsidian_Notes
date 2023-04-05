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
await kernel.Memory.SaveReferenceAsync(
	id: "INTERNAL_ID"
    collection: "COLLECTION_NAME",  
    description: "DESCRIPTION",  
    text: "TEXT",  
    externalId: "https://source/item/id",  
    externalSourceName: "source" 
);
```

Retrieve data by ID
```csharp
MemoryQueryResult? lookup = await kernel.Memory.GetAsync("COLLECTION_NAME", "INTERNAL_ID");
```

Search Data by vector similarity
```csharp
var searchResults = kernel.Memory.SearchAsync("COLLECTION_NAME", "some query input", limit: 3, minRelevanceScore: 0.8);

await foreach (var item in searchResults)  
{  
    Console.WriteLine(item.Metadata.Text + " : " + item.Relevance);  
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


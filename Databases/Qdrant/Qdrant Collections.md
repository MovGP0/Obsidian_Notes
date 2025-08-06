> [!info]
> [Documenation: Collections](https://qdrant.tech/documentation/concepts/collections/)

## Create a collection

```csharp
await client.CreateCollectionAsync(
    // name of the collection
	collectionName: "example_collection",
	vectorsConfig: new VectorParams
	{
	    // number of dimensions of the index vectors
	    Size = 4,

	    // distance function to use
	    Distance = Distance.Dot,

	    // use multiple named indices
        Map =
		{
			["image"] = new VectorParams
			{
				Size = 4,
				Distance = Distance.Dot
			},
			["text"] = new VectorParams
			{
				Size = 8,
				Distance = Distance.Cosine,
				Datatype = Datatype.Uint8
			},
		}
	},
	// use sparse vectors insteaf of dense vectors for index "text"
	sparseVectorsConfig: ("text", new SparseVectorParams(),

    optimizersConfig: new OptimizersConfigDiff
    {
	    IndexingThreshold = 10000
	 },

	quantizationConfig: new QuantizationConfigDiff
	{
		Scalar = new ScalarQuantization
		{
			Type = QuantizationType.Int8,
			Quantile = 0.8f,
			AlwaysRam = true
		}
	}
);
```

## Check if collection exists

```csharp
await client.CollectionExistsAsync("example_collection");
```

## Delete a collection

```csharp
await client.DeleteCollectionAsync("example_collection");
```

## List all collections

```csharp
await client.ListCollectionsAsync();
```

## Alias for collection

Create alias:
```csharp
await client.CreateAliasAsync(
	aliasName: "production_collection",
	collectionName: "example_collection");
```

Delete alias:
```csharp
await client.DeleteAliasAsync("example_collection");
```

List alias of a collection:
```csharp
await client.ListCollectionAliasesAsync("example_collection");
```

List all alias:
```csharp
await client.ListAliasesAsync();
```

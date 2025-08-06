> [!info]
> [Documentation: Search](https://qdrant.tech/documentation/concepts/search/)

## Basic Query
```csharp
var searchResult = await client.QueryAsync(
    collectionName: "example_collection",
    query: [0.2f, 0.1f, 0.9f, 0.7f],
    limit: 3,
    payloadSelector: true, // take all (default)
    vectorsSelector: true // take all (default)
);
```

## Advanced Queries
```csharp
// advanced queries
var searchResult = await client.QueryAsync(
    collectionName: "example_collection",
    query: new float[] { 0.2f, 0.1f, 0.9f, 0.7f },
    filter: MatchKeyword("city", "London"),
    searchParams: new SearchParams { Exact = false, HnswEf = 128 },
    // when collection has named vector indices
    usingVector: "nomic-embed-text",
    // include/exclude payload-fields from the result
    payloadSelector: new WithPayloadSelector
	{
		Exclude = new PayloadExcludeSelector { Fields = { new string[] { "city" } } }
	},
	vectorsSelector: true,

    // Pagination: skip first 10 results
    offset: 100,
    // Pagination: take 10 results (pagination)
    limit: 10,

    // Grouping: group by field
    groupBy: "categoryId",
    // Grouping: how many results per group
    groupSize: 10,

    // Join with other collection
    // will join by the given group (e.g. "categoryId")
    withLookup: new WithLookup
    {
        Collection = "documents",
        WithPayload = new WithPayloadSelector
        {
            Include = new PayloadIncludeSelector { Fields = { new string[] { "title", "text" } } }
        },
        WithVectors = false
    }
);
```
## Batched Queries
```csharp
var queries = new List<QueryPoints>();
queries.Add(query0);
queries.Add(query1);
// ...

var searchResult = await client.QueryBatchAsync(collectionName: "example_collection", queries: queries);
```

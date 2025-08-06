> [!info]
> [Documentation: Points](https://qdrant.tech/documentation/concepts/points/)

> [!Note]
> **Points** contain a unique id, the index vectors, and a data payload

> [!Note]
> The `Id` of a point can be either a 64 bit unsigned int or a `UUID` (or a `Guid`)

## Create points

```csharp
var points = new PointStruct() { Id = 1, Vectors = [0.05f, 0.61f, 0.76f, 0.74f], Payload = { ["city"] = "Berlin" } },
```

## Create points with multiple/named vector indices

```csharp
var point = new PointStruct()
{
    Id = 1,
    Vectors = new Dictionary<string, float[]>
	{
		["image"] = [0.9f, 0.1f, 0.1f, 0.2f],
		["text"] = [0.4f, 0.7f, 0.1f, 0.8f, 0.1f, 0.1f, 0.9f, 0.2f]
	}
}
```

## Create points with with sparse vectors

**Example:** Indices `0..5` and `8..` are set to `0.0f`
```csharp
var point = new PointStruct()
{
	Id = 1,
	Vectors = new Dictionary<string, Vector> { ["text"] = ([1.0f, 2.0f], [6, 7]) }
},
```

## Add points to a collection

```csharp
// add points with vectors
var operationInfo = await client.UpsertAsync(
    collectionName: "example_collection",
    points: new List<PointStruct>
    {
        point0,
        point1,
        point2,
        // ...
    });

// operationInfo = { "operationId": "0", "status": "Completed" }
```

## Query points

```csharp
await client.RetrieveAsync(
	collectionName: "{collection_name}",
	ids: [0, 30, 100],
	withPayload: false,
	withVectors: false
);
```

## Delete points from a collection

Deletes the points with the specified id's:
```csharp
await client.DeleteAsync(collectionName: "example_collection", ids: [0, 3, 100]);
```

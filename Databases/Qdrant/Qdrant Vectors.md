> [!Info]
> [Documentation: Vectors](https://qdrant.tech/documentation/concepts/vectors/)

## Add or update vectors to points

Updates the vectors of the specified points:
```csharp
await client.UpdateVectorsAsync(
    collectionName: "example_collection",
    points: new List<PointStruct>
    {
        point0,
        point1,
        point2,
        // ...
    });
)
```

## Delete vectors from points

Deletes the vectors of the specified points:
```csharp
await client.DeleteVectorsAsync("example_collection", ["text", "image"], [0, 3, 10]);
```

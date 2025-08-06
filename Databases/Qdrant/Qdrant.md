---
tags:
  - Vector-Database
---
Setting up via Docker:
```sh
docker pull qdrant/qdrant
docker run -p 6333:6333 -p 6334:6334 -v "$(pwd)/qdrant_storage:/qdrant/storage:z" qdrant/qdrant
```

- REST API: [localhost:6333](http://localhost:6333/)
- Web UI: [localhost:6333/dashboard](http://localhost:6333/dashboard)
- GRPC API: [localhost:6334](http://localhost:6334/)

## Connect via .NET

Install NuGet:
```sh
dotnet add package Qdrant.Client
```

Connect to database:
```csharp
// connect via GRPC
var client = new QdrantClient("localhost", 6334);
```

Next steps:
- [[Qdrant Collections|Create a collection]]
- [[Qdrant Points|Add data-points]]
	- [[Qdrant Vectors|manage Vectors]]
	- [[Qdrant Payload|manage Payload]]
- [[Qdrant Queries|Query the data]]

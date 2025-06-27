## Indexing

- Use Roslyn to parse source code
	- Extract all .NET entities
	  Assembly, Namespace, Class, Interface, Struct, Record, Enum, Attribute, Field, Property, Method/Constructor, Event, etc.
- Create embeddings for source code and store in vector database
	- Consider using [Qdrant](https://qdrant.tech/) or [Milvus](https://milvus.io/) as vector database
		- Qdrant is better for small projects
		- Milvus is better for enterprise projects where easy scaling and/or a permission system is required
		- Pinecone is a commercial cloud alternative
- Represent relationships between objects in a graph database
	- Consider use UML notation/wordings to represent relationships on the graph
	- Consider using Neo4J as graph database
- Each object needs to be identified in the vector database and the graph database with the same id
	- The id should represent the .NET-Path of the object
	  Ie. something like `[AssemblyName]Full.Namespace.ClassName{T}.SomeMethodNameAsync{T,V}(T,string,CancellationToken)` for methods
- For cost considerations, we should be able to use a local LLM instead of OpenAI
	- [LLM Studio](https://lmstudio.ai/) or [ollama](https://ollama.com/)
	- Note that those solutions do not support the full API of OpenAI's OpenAPI
- There might be the need for storing chats and embeddings
	- Consider using DuckDB, Postgres, SQLite, or Redis

## Graph Inference

There should be an inference job that enriches the information in the vector database. This is required for tracking afferent and efferent couplings of different objects.

Inferred relations should refer to the relations from which they have been inferred from.

**Examples:**
- If A implements B and B implements C, than A implements C
- If A uses B, then B is used by A

Note that some attributes are inherited, while others aren't inherited.

## Retrieval

When the user asks a question, the relevant code should be retrieved

- Find code that relates to the user query in the vector database
- Retrieve graph information for the relevant code from the graph database and add to the response of the user

## Presentation

There should be a REST endpoint where the user can send their query in text form and get a JSON response with data relevant to the user query
- relevant code snippets
	- Location (id) of the object
	- Source Code
	- UML relationships with other code
	- [[Metrics of Object Oriented Software|Metrics]] of the code snippet
- LLM generated summary of search results
	- May be generated using cached embeddings, such that they do not need to be re-calculated every time.
	- **Note** the embedding used for search may differ from the embedding required for answer generation.
		- The used embedding algorithm should be stored as attribute in the vector database.
		- There might be multiple embeddings that need to be generated and stored in a database.
		- The additional embeddings might be stored in a database that differs from the vector database (ie. a key-value store or relational database).

## LLM Studio

The LLM Studio API supports the following endpoints:
```http
GET /v1/models
POST /v1/chat/completions
POST /v1/completions
POST /v1/embeddings
```

## OpenAI OpenAPI

See [OpenAI OpenAPI Swagger](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/openai/openai-openapi/refs/heads/master/openapi.yaml#/) for the supported endpoints.

## NuGet Packages

Code ingestion

- `Microsoft.CodeAnalysis`
- `Microsoft.CodeAnalysis.CSharp`
- `Microsoft.CodeAnalysis.Workspaces.MSBuild`

Persistence layer

- `Neo4j.Driver` (Neo4J support)
- `Microsoft.SemanticKernel`
- `Microsoft.SemanticKernel.Connectors`
- `Microsoft.SemanticKernel.Connectors.OpenAI` (Support for OpenAI functionality)
- `Microsoft.SemanticKernel.Connectors.Qdrant` (Qdrant as vector store)
	- `Qdrant.Client`
- `Microsoft.SemanticKernel.Connectors.Milvus` (Milvus as vector store)
- `Microsoft.SemanticKernel.Connectors.Pinecone` (Pinecone as vector store)
- `Microsoft.SemanticKernel.Connectors.DuckDB` (DuckDB for embedding storage)
- `Microsoft.SemanticKernel.Plugins.Memory` (Vector Stores)
- `Microsoft.SemanticKernel.Plugins.OpenApi` (OpenAPI compatible endpoints like OpenAI or LLM Studio)

**Note:** SemanticKernel does not natively support graph databases yet.

## Roslyn

```csharp
string projectPath = @"C:\Path\To\YourProject.csproj";

// Initialize Roslyn workspace
using var workspace = MSBuildWorkspace.Create();
var project = await workspace.OpenProjectAsync(projectPath);
var compilation = await project.GetCompilationAsync();

foreach (var syntaxTree in compilation.SyntaxTrees)
{
    var semanticModel = compilation.GetSemanticModel(syntaxTree);
	// ...
}
```

See [[Roslyn]] for details.

## Mapping to UML

The objects in the syntax tree should be mapped to UML
- **Essential Entities**: Classes, Interfaces, Enums, and Packages.
- **Entity Properties**: Names, stereotypes, attributes, operations, visibilities, etc.
- **Core Relationships**: Generalization, Realization, Association, Aggregation, Composition, Dependency.
- **Relationship Properties**: Multiplicity, navigability, roles, stereotypes, and possibly lifecycle implications (especially for composition).

**Note:** UML does not support all features that are available in C#. Additional attributes may be required to represent those discrepancies.

## Neo4J

Initialize Neo4j connection
```csharp
var driver = GraphDatabase.Driver("bolt://localhost:7687", AuthTokens.Basic("neo4j", "password"));
var session = driver.AsyncSession();

// use Neo4J database here

// dispose
await session.CloseAsync();
await driver.CloseAsync();
```

Clear all data / delete database
```csharp
await session.RunAsync("MATCH (n) DETACH DELETE n");
```

Create simple nodes
```csharp
await session.RunAsync("MERGE (c:Class {id: $id})", new { id = classId });
await session.RunAsync("MERGE (m:Method {id: $id})", new { id = methodId });
```

Create complex nodes
```csharp
var createClassesQuery = @"
	CREATE (customer:Class {
		name: 'Customer',
		stereotype: 'entity',
		isAbstract: false
	})
	CREATE (order:Class {
		name: 'Order',
		stereotype: 'entity',
		isAbstract: false
	})
	RETURN customer, order
";

var createResult = await session.RunAsync(createClassesQuery);
```

Create simple vertex
```csharp
await session.RunAsync(@"
	MATCH (c:Class {id: $classId})
	MATCH (m:Method {id: $methodId})
	MERGE (c)-[:CONTAINS]->(m)",
	new { classId, methodId });
```

Create complex vertex
```csharp
var createAssociationQuery = @"
	MATCH (c:Class {name: 'Customer'})
	MATCH (o:Class {name: 'Order'})
	CREATE (c)-[:ASSOCIATION {
		multiplicity: '1..*',
		navigability: 'bidirectional'
	}]->(o)
	RETURN c, o
";

var assocResult = await session.RunAsync(createAssociationQuery);
```

Retrieve incoming and outgoing relations of an object with a given ID
```csharp
var objectId = ...;

// Define the Cypher query
// - `MATCH (n)-[r]->(o)` retrieves outgoing relationships from the node `n` to other nodes.
// - `MATCH (n)<-[r]-(o)` retrieves incoming relationships from other nodes to `n`.
// - The `UNION` combines the results.
var query = @"
MATCH (n)-[r]->(o)
WHERE id(n) = $id
RETURN n, r, o
UNION
MATCH (n)<-[r]-(o)
WHERE id(n) = $id
RETURN n, r, o";

// Execute the query
var result = await session.RunAsync(query, new { id = objectId });

// Process the results
await foreach (var record in result)
{
	var node = record["n"].As<INode>();
	var relationship = record["r"].As<IRelationship>();
	var connectedNode = record["o"].As<INode>();

	Console.WriteLine($"Node: {node.Id} - {node.Labels[0]}");
	Console.WriteLine($"Relationship: {relationship.Type}");
	Console.WriteLine($"Connected Node: {connectedNode.Id} - {connectedNode.Labels[0]}");
	Console.WriteLine(new string('-', 50));
}
```

## Qdrant

Create connection
```csharp
var qdrantClient = new QdrantClient("http://localhost:6333");
var vectorStore = new QdrantVectorStore(qdrantClient);
```

Create collection
```csharp
var collectionName = "my_collection_name";
var collection = vectorStore.GetCollection</*id*/string, /*record*/VectorStoreRecord>(collectionName);
await collection.CreateCollectionIfNotExistsAsync();
```

Create embedding
```csharp
var embeddingService = ...;
var embedding = await embeddingService.GenerateEmbeddingAsync(text);
```

Store record in database
```csharp
var record = new VectorStoreRecord
{
    Id = objectId,
    Vector = embedding,
    Payload = new Dictionary<string, object>
    {
        { "source", sourceCode },
        // other properties
    }
};

await collection.UpsertAsync(record);
```

Perform a vector search
```csharp
var query = "Query text";
var queryEmbedding = await embeddingService.GenerateEmbeddingAsync(query);

var searchOptions = new VectorSearchOptions
{
    Top = 5
};

var results = await collection.VectorizedSearchAsync(queryEmbedding, searchOptions);

await foreach (var result in results.Results)
{
    Console.WriteLine($"Text: {result.Record.Payload["source"]}");
    Console.WriteLine($"Score: {result.Score}");
}
```

| Measurement            | Description                                                                                                   | Formula                                                                                                                             |
| ---------------------- | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Degree                 | Number of edges connected to a vertex.                                                                        | $\deg(v) = \sum_{u \in V} a_{uv}$                                                                                                   |
| Average Degree         | Average number of edges connected to vertices.                                                                | $\langle k \rangle = \frac{2E}{N}$                                                                                                  |
| Density                | Ratio of the number of edges to the possible number of edges.                                                 | $D = \frac{2E}{N(N-1)}$                                                                                                             |
| Diameter               | Longest shortest path between any two vertices.                                                               | $\text{diam}(G) = \max_{u,v \in V} d(u,v)$                                                                                          |
| Radius                 | Minimum eccentricity of any vertex.                                                                           | $\text{rad}(G) = \min_{v \in V} \text{ecc}(v)$                                                                                      |
| Eccentricity           | Maximum distance from a vertex to any other vertex.                                                           | $\text{ecc}(v) = \max_{u \in V} d(v,u)$                                                                                             |
| Clustering Coefficient | Measure of the degree to which vertices tend to cluster together.                                             | $C = \frac{3 \times \text{number of triangles}}{\text{number of connected triples}}$                                                |
| Betweenness Centrality | Measure of the number of times a vertex acts as a bridge along the shortest path between two other vertices.  | $C_B(v) = \sum_{s \neq v \neq t} \frac{\sigma_{st}(v)}{\sigma_{st}}$                                                                |
| Closeness Centrality   | Measure of how close a vertex is to all other vertices.                                                       | $C_C(v) = \frac{N-1}{\sum_{u \in V} d(v,u)}$                                                                                        |
| Eigenvector Centrality | Measure of the influence of a vertex in a network.                                                            | $x_v = \frac{1}{\lambda} \sum_{u \in V} a_{uv} x_u$                                                                                 |
| PageRank               | Measure of the importance of a vertex based on its connections.                                               | $PR(v) = \frac{1-d}{N} + d \sum_{u \in \text{adj}(v)} \frac{PR(u)}{\deg(u)}$                                                        |
| Adjacency Matrix       | Matrix representation of the graph where \(a_{uv} = 1\) if there is an edge from \(u\) to \(v\), otherwise 0. | $A = [a_{uv}]$                                                                                                                      |
| Incidence Matrix       | Matrix representation where rows represent vertices and columns represent edges.                              | $B = [b_{ve}] \text{ where } b_{ve} = \begin{cases} 1 & \text{if } v \text{ is incident to } e \\ 0 & \text{otherwise} \end{cases}$ |

## Using `QuickGraph`

Setting up a graph
```csharp
using System;
using QuikGraph;
using QuikGraph.Algorithms;

var graph = new UndirectedGraph<int, Edge<int>>();

// Adding vertices
for (int i = 1; i <= 5; i++)
{
	graph.AddVertex(i);
}

// Adding edges
graph.AddEdge(new Edge<int>(1, 2));
graph.AddEdge(new Edge<int>(1, 3));
graph.AddEdge(new Edge<int>(2, 4));
graph.AddEdge(new Edge<int>(3, 4));
graph.AddEdge(new Edge<int>(4, 5));
```

### Degree

The degree of a vertex can be calculated using the `Degree` method:
```csharp
foreach (var vertex in graph.Vertices)
{
    Console.WriteLine($"Degree of vertex {vertex}: {graph.AdjacentDegree(vertex)}");
}
```

### Average Degree

The average degree can be calculated as follows:
```csharp
int totalDegree = graph.Vertices.Sum(v => graph.AdjacentDegree(v));
double averageDegree = (double)totalDegree / graph.VertexCount;

Console.WriteLine($"Average Degree: {averageDegree}");
```

### Density

Graph density can be calculated using:
```csharp
int vertexCount = graph.VertexCount;
int edgeCount = graph.EdgeCount;
double density = (2.0 * edgeCount) / (vertexCount * (vertexCount - 1));

Console.WriteLine($"Density: {density}");
```

### Diameter, Radius, and Eccentricity

Calculating the diameter and radius requires finding the shortest paths between all pairs of vertices.

QuikGraph doesn't directly provide these, but you can use the [Floyd-Warshall algorithm](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm) to calculate all pairs shortest paths, then determine the diameter and radius from these.

```csharp
using QuikGraph.Algorithms.ShortestPath;
```

```csharp
var floydWarshall = new FloydWarshallAllShortestPathAlgorithm<int, Edge<int>>(graph, e => 1.0);
floydWarshall.Compute();

double diameter = 0;
double radius = double.MaxValue;

foreach (var v in graph.Vertices)
{
    double eccentricity = 0;
    foreach (var u in graph.Vertices)
    {
        if (v != u)
        {
            double distance = floydWarshall.Distances[(v, u)];
            if (distance > eccentricity)
            {
                eccentricity = distance;
            }
        }
    }

    if (eccentricity > diameter)
    {
        diameter = eccentricity;
    }
    if (eccentricity < radius)
    {
        radius = eccentricity;
    }

	Console.WriteLine($"Eccentricity of vertex {v}: {eccentricity}");
}

Console.WriteLine($"Diameter: {diameter}");
Console.WriteLine($"Radius: {radius}");
```

### Clustering Coefficient

QuikGraph does not have a built-in method for calculating the clustering coefficient, but you can implement it manually:

```csharp
double clusteringCoefficient = 0;

foreach (var vertex in graph.Vertices)
{
    var neighbors = graph.AdjacentEdges(vertex).Select(e => e.GetOtherVertex(vertex)).ToList();
    int neighborCount = neighbors.Count;
    if (neighborCount < 2)
    {
        continue;
    }

    int actualEdgesBetweenNeighbors = 0;

    for (int i = 0; i < neighborCount; i++)
    {
        for (int j = i + 1; j < neighborCount; j++)
        {
            if (graph.ContainsEdge(neighbors[i], neighbors[j]))
            {
                actualEdgesBetweenNeighbors++;
            }
        }
    }

    double possibleEdgesBetweenNeighbors = neighborCount * (neighborCount - 1) / 2.0;
    clusteringCoefficient += actualEdgesBetweenNeighbors / possibleEdgesBetweenNeighbors;
}

clusteringCoefficient /= graph.VertexCount;

Console.WriteLine($"Clustering Coefficient: {clusteringCoefficient}");
```

### Betweenness Centrality

QuikGraph does not have a direct method for Betweenness Centrality, but you can implement it manually or use another library.

```csharp
using QuikGraph;
using QuikGraph.Algorithms;
using QuikGraph.Algorithms.Observers;
using QuikGraph.Algorithms.ShortestPath;
```

Implementation:
```csharp
static Dictionary<int, double> CalculateBetweennessCentrality(UndirectedGraph<int, Edge<int>> graph)
{
	var betweenness = new Dictionary<int, double>();
	foreach (var v in graph.Vertices)
	{
		betweenness[v] = 0.0;
	}

	foreach (var s in graph.Vertices)
	{
		var stack = new Stack<int>();
		var predecessors = new Dictionary<int, List<int>>();
		var shortestPaths = new Dictionary<int, double>();
		var numShortestPaths = new Dictionary<int, double>();
		var dependency = new Dictionary<int, double>();

		foreach (var v in graph.Vertices)
		{
			predecessors[v] = new List<int>();
			shortestPaths[v] = -1;
			numShortestPaths[v] = 0;
			dependency[v] = 0.0;
		}

		shortestPaths[s] = 0;
		numShortestPaths[s] = 1;
		var queue = new Queue<int>();
		queue.Enqueue(s);

		while (queue.Count > 0)
		{
			var v = queue.Dequeue();
			stack.Push(v);

			foreach (var edge in graph.AdjacentEdges(v))
			{
				var w = edge.GetOtherVertex(v);
				if (shortestPaths[w] < 0)
				{
					queue.Enqueue(w);
					shortestPaths[w] = shortestPaths[v] + 1;
				}

				if (shortestPaths[w] == shortestPaths[v] + 1)
				{
					numShortestPaths[w] += numShortestPaths[v];
					predecessors[w].Add(v);
				}
			}
		}

		while (stack.Count > 0)
		{
			var w = stack.Pop();
			foreach (var v in predecessors[w])
			{
				dependency[v] += (numShortestPaths[v] / numShortestPaths[w]) * (1 + dependency[w]);
			}
			if (w != s)
			{
				betweenness[w] += dependency[w];
			}
		}
	}

	// Normalize the betweenness centrality values
	int n = graph.VertexCount;
	foreach (var v in graph.Vertices)
	{
		betweenness[v] /= 2.0; // For undirected graph
	}

	return betweenness;
}
```

Usage:
```csharp
// Create the graph
var graph = new UndirectedGraph<int, Edge<int>>();

// Adding vertices
for (int i = 1; i <= 5; i++)
{
	graph.AddVertex(i);
}

// Adding edges
graph.AddEdge(new Edge<int>(1, 2));
graph.AddEdge(new Edge<int>(1, 3));
graph.AddEdge(new Edge<int>(2, 4));
graph.AddEdge(new Edge<int>(3, 4));
graph.AddEdge(new Edge<int>(4, 5));

// Calculate Betweenness Centrality
var betweennessCentrality = CalculateBetweennessCentrality(graph);

// Display results
foreach (var kvp in betweennessCentrality)
{
	Console.WriteLine($"Betweenness Centrality of vertex {kvp.Key}: {kvp.Value}");
}
```

### Closeness Centrality

Closeness centrality can be computed using the shortest paths:
```csharp
foreach (var vertex in graph.Vertices)
{
    double closenessCentrality = 0;
    double sumDistances = 0;

    foreach (var otherVertex in graph.Vertices)
    {
        if (vertex != otherVertex)
        {
            sumDistances += floydWarshall.Distances[(vertex, otherVertex)];
        }
    }

    closenessCentrality = (graph.VertexCount - 1) / sumDistances;

    Console.WriteLine($"Closeness Centrality of vertex {vertex}: {closenessCentrality}");
}
```

### Eigenvector Centrality

```csharp
using MathNet.Numerics.LinearAlgebra;
using QuikGraph;
using QuikGraph.Algorithms;
```

Implementation using `Math.NET`:
```csharp
static double[] CalculateEigenvectorCentrality(Matrix<double> adjacencyMatrix)
{
	// Calculate the largest eigenvalue and corresponding eigenvector
	var evd = adjacencyMatrix.Evd();
	var eigenValues = evd.EigenValues.Real();
	var eigenVectors = evd.EigenVectors;
	
	// Find the index of the largest eigenvalue
	int maxIndex = eigenValues.MaximumIndex();
	
	// Get the corresponding eigenvector
	var centralityVector = eigenVectors.Column(maxIndex);
	
	// Normalize the eigenvector
	var normCentralityVector = centralityVector.Normalize(2.0);
	
	return normCentralityVector.ToArray();
}
```

Usage:
```csharp
// Create the graph
var graph = new UndirectedGraph<int, Edge<int>>();

// Adding vertices
for (int i = 1; i <= 5; i++)
{
	graph.AddVertex(i);
}

// Adding edges
graph.AddEdge(new Edge<int>(1, 2));
graph.AddEdge(new Edge<int>(1, 3));
graph.AddEdge(new Edge<int>(2, 4));
graph.AddEdge(new Edge<int>(3, 4));
graph.AddEdge(new Edge<int>(4, 5));

// Get the adjacency matrix
var adjacencyMatrix = GetAdjacencyMatrix(graph);

// Calculate Eigenvector Centrality
var eigenvectorCentrality = CalculateEigenvectorCentrality(adjacencyMatrix);

// Display results
foreach (var vertex in graph.Vertices)
{
	Console.WriteLine($"Eigenvector Centrality of vertex {vertex}: {eigenvectorCentrality[vertex - 1]}");
}
```

### PageRank

You can use the PageRank algorithm provided by `QuikGraph`:

```csharp
var pageRank = graph.PageRank(0.85);
foreach (var vertex in pageRank)
{
    Console.WriteLine($"PageRank of vertex {vertex.Key}: {vertex.Value}");
}
```

### Adjacency Matrix

The adjacency matrix can be constructed as follows:
```csharp
static int[,] GetAdjacencyMatrix(UndirectedGraph<int, Edge<int>> graph)
{
	int[,] adjacencyMatrix = new int[graph.VertexCount, graph.VertexCount];
	var vertexIndices = graph.Vertices.Select((v, i) => new { v, i }).ToDictionary(vi => vi.v, vi => vi.i);
	
	foreach (var edge in graph.Edges)
	{
	    int i = vertexIndices[edge.Source];
	    int j = vertexIndices[edge.Target];
	    adjacencyMatrix[i, j] = 1;
	    adjacencyMatrix[j, i] = 1; // for undirected graph
	}
	
	// Display adjacency matrix
	for (int i = 0; i < graph.VertexCount; i++)
	{
	    for (int j = 0; j < graph.VertexCount; j++)
	    {
	        Console.Write(adjacencyMatrix[i, j] + " ");
	    }
	    Console.WriteLine();
	}

	return adjacencyMatrix;
}
```

Using `Math.NET`:
```csharp
static Matrix<double> GetAdjacencyMatrix(UndirectedGraph<int, Edge<int>> graph)
{
	int vertexCount = graph.VertexCount;
	var adjacencyMatrix = Matrix<double>.Build.Dense(vertexCount, vertexCount);
	
	var vertexIndices = graph.Vertices.Select((v, i) => new { v, i }).ToDictionary(vi => vi.v, vi => vi.i);
	
	foreach (var edge in graph.Edges)
	{
		int i = vertexIndices[edge.Source];
		int j = vertexIndices[edge.Target];
		adjacencyMatrix[i, j] = 1;
		adjacencyMatrix[j, i] = 1; // for undirected graph
	}
	
	return adjacencyMatrix;
}
```

### Incidence Matrix

The incidence matrix can be constructed as follows:

```csharp
int[,] incidenceMatrix = new int[graph.VertexCount, graph.EdgeCount];
var edgeIndices = graph.Edges.Select((e, i) => new { e, i }).ToDictionary(ei => ei.e, ei => ei.i);

foreach (var edge in graph.Edges)
{
    int v = vertexIndices[edge.Source];
    int w = vertexIndices[edge.Target];
    int e = edgeIndices[edge];

    incidenceMatrix[v, e] = 1;
    incidenceMatrix[w, e] = 1; // for undirected graph
}

// Display incidence matrix
for (int i = 0; i < graph.VertexCount; i++)
{
    for (int j = 0; j < graph.EdgeCount; j++)
    {
        Console.Write(incidenceMatrix[i, j] + " ");
    }
    Console.WriteLine();
}
```

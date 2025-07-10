Graph is probably the data structure that has the closest resemblance to our daily life. There are many types of graphs describing the relationships in real life.

A Graph is a non-linear data structure consisting of vertices and edges. The vertices are sometimes also referred to as nodes and the edges are lines or arcs that connect any two nodes in the graph. More formally a Graph is composed of a set of vertices(V) and a set of edges(E). The graph is denoted by G(V, E).

An adjacency matrix is a way of representing a graph as a matrix of boolean (0's and 1's)

Let's assume there are n vertices in the graph So, create a 2D matrix adjMat[n][n] having dimension n x n.

If there is an edge from vertex i to j, mark adjMat[i][j] as 1. 
If there is no edge from vertex i to j, mark adjMat[i][j] as 0.

An array of Lists is used to store edges between two vertices. The size of array is equal to the number of vertices (i.e, n). Each index in this array represents a specific vertex in the graph. The entry at the index i of the array contains a linked list containing the vertices that are adjacent to vertex i. Let's assume there are n vertices in the graph So, create an array of list of size n as adjList[n].

adjList[0] will have all the nodes which are connected (neighbour) to vertex 0.
adjList[1] will have all the nodes which are connected (neighbour) to vertex 1 and so on.

Undirected graphs
The edges between any two vertices in an “undirected graph” do not have a direction, indicating a two-way relationship.

Directed graphs
The edges between any two vertices in a “directed graph” graph are directional.

Weighted graphs
Each edge in a “weighted graph” has an associated weight. The weight can be of any metric, such as time, distance, size, etc. The most commonly seen “weighted map” in our daily life might be a city map.

Edge: The connection between two vertices are the edges of the graph.

Path: the sequence of vertices to go through from one vertex to another.

Path Length: the number of edges in a path.

Cycle: a path where the starting point and endpoint are the same vertex.

Negative Weight Cycle: In a “weighted graph”, if the sum of the weights of all edges of a cycle is a negative value, it is a negative weight cycle.

Connectivity: if there exists at least one path between two vertices, these two vertices are connected.

Degree of a Vertex: the term “degree” applies to unweighted graphs. The degree of a vertex is the number of edges connecting the vertex.

In-Degree: “in-degree” is a concept in directed graphs. If the in-degree of a vertex is d, there are d directional edges incident to the vertex.

Out-Degree: “out-degree” is a concept in directed graphs. If the out-degree of a vertex is d, there are d edges incident from the vertex.



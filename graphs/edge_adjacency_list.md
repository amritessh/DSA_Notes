🎯 Edge List vs Adjacency List: Parameter Differences
🚨 The Confusion: Same Data Type, Different Meaning!
Both functions use vector<vector<int>> but the CONTENT is completely different:
Function 1: Edge List Parameter
cppvector<vector<int>> edgesToAdjList(vector<vector<int>>& edges, int n)
//                                 ^^^^^^^^^^^^^^^^^^^^^^^^^
//                                 This is an EDGE LIST
Function 2: Adjacency List Parameter
cppvector<vector<int>> adjListToEdges(vector<vector<int>>& adj)
//                                 ^^^^^^^^^^^^^^^^^^^^^^
//                                 This is an ADJACENCY LIST
📊 Visual Comparison
Same Graph:
    0 ─── 1
    │     │
    │     │
    2 ─── 3
Edge List (Function 1 Parameter):
cppvector<vector<int>> edges = {
    {0, 1},    // Edge between vertex 0 and vertex 1
    {0, 2},    // Edge between vertex 0 and vertex 2
    {1, 3},    // Edge between vertex 1 and vertex 3
    {2, 3}     // Edge between vertex 2 and vertex 3
};
What each row means:

edges[0] = {0, 1} → "There's an edge connecting vertex 0 to vertex 1"
edges[1] = {0, 2} → "There's an edge connecting vertex 0 to vertex 2"
Each row represents ONE EDGE (2 vertices)

Adjacency List (Function 2 Parameter):
cppvector<vector<int>> adj = {
    {1, 2},    // Vertex 0 connects to vertices 1 and 2
    {0, 3},    // Vertex 1 connects to vertices 0 and 3
    {0, 3},    // Vertex 2 connects to vertices 0 and 3
    {1, 2}     // Vertex 3 connects to vertices 1 and 2
};
What each row means:

adj[0] = {1, 2} → "Vertex 0's neighbors are vertices 1 and 2"
adj[1] = {0, 3} → "Vertex 1's neighbors are vertices 0 and 3"
Each row represents ONE VERTEX and its neighbors

🔍 Key Differences
AspectEdge ListAdjacency ListData Typevector<vector<int>>vector<vector<int>>Row MeaningEach row = 1 EDGEEach row = 1 VERTEXRow Content{u, v} = edge from u to v{n1, n2, ...} = neighborsRow SizeUsually 2 (source, destination)Variable (number of neighbors)Array SizeNumber of edgesNumber of verticesAccess Patternedges[i][0] and edges[i][1]adj[vertex] gives all neighbors
🎭 Detailed Examples
Edge List Example:
cppvector<vector<int>> edges = {{0,1}, {1,2}, {2,0}};

// How to read this:
// edges[0] = {0, 1} → Edge from vertex 0 to vertex 1
// edges[1] = {1, 2} → Edge from vertex 1 to vertex 2
// edges[2] = {2, 0} → Edge from vertex 2 to vertex 0

// To process all edges:
for (auto& edge : edges) {
    int u = edge[0];  // Source vertex
    int v = edge[1];  // Destination vertex
    cout << "Edge: " << u << " -> " << v << endl;
}
Adjacency List Example:
cppvector<vector<int>> adj = {{1,2}, {0,2}, {0,1}};

// How to read this:
// adj[0] = {1, 2} → Vertex 0 connects to vertices 1 and 2
// adj[1] = {0, 2} → Vertex 1 connects to vertices 0 and 2
// adj[2] = {0, 1} → Vertex 2 connects to vertices 0 and 1

// To process all vertices and their neighbors:
for (int vertex = 0; vertex < adj.size(); vertex++) {
    cout << "Vertex " << vertex << " neighbors: ";
    for (int neighbor : adj[vertex]) {
        cout << neighbor << " ";
    }
    cout << endl;
}


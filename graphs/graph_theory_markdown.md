# Complete Graph Theory Revision Guide

## Table of Contents
1. [Disjoint Set (Union-Find)](#1-disjoint-set-union-find)
2. [Depth First Search (DFS)](#2-depth-first-search-dfs)
3. [Breadth First Search (BFS)](#3-breadth-first-search-bfs)
4. [Minimum Spanning Tree](#4-minimum-spanning-tree)
5. [Single Source Shortest Path](#5-single-source-shortest-path)
6. [Topological Sorting (Kahn's Algorithm)](#6-topological-sorting-kahns-algorithm)

---

# 1. Disjoint Set (Union-Find)

## Theory
**Disjoint Set** is a data structure that keeps track of elements partitioned into disjoint (non-overlapping) subsets. It efficiently supports two operations:
- **Find:** Determine which subset an element belongs to
- **Union:** Merge two subsets into one

## Key Concepts
- **Path Compression:** Make nodes point directly to root during Find
- **Union by Rank/Size:** Attach smaller tree under larger tree
- **Applications:** Kruskal's MST, Connected Components, Cycle Detection

## Templates

### Basic Union-Find
```cpp
class UnionFind {
private:
    vector<int> parent, rank;
    int components;
    
public:
    UnionFind(int n) : parent(n), rank(n, 0), components(n) {
        iota(parent.begin(), parent.end(), 0); // parent[i] = i
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    bool unite(int x, int y) {
        int rootX = find(x), rootY = find(y);
        if (rootX == rootY) return false; // Already connected
        
        // Union by rank
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
        
        components--;
        return true;
    }
    
    bool connected(int x, int y) {
        return find(x) == find(y);
    }
    
    int getComponents() { return components; }
};
```

### Union-Find with Size Tracking
```cpp
class UnionFindSize {
private:
    vector<int> parent, size;
    
public:
    UnionFindSize(int n) : parent(n), size(n, 1) {
        iota(parent.begin(), parent.end(), 0);
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    bool unite(int x, int y) {
        int rootX = find(x), rootY = find(y);
        if (rootX == rootY) return false;
        
        // Union by size
        if (size[rootX] < size[rootY]) {
            parent[rootX] = rootY;
            size[rootY] += size[rootX];
        } else {
            parent[rootY] = rootX;
            size[rootX] += size[rootY];
        }
        return true;
    }
    
    int getSize(int x) { return size[find(x)]; }
};
```

## Example Problems
- **Number of Connected Components in Undirected Graph**
- **Accounts Merge**
- **Friend Circles**
- **Redundant Connection**

## Time Complexity
- **Find:** O(α(n)) - nearly constant with path compression
- **Union:** O(α(n)) - nearly constant with union by rank
- **α(n):** Inverse Ackermann function, effectively constant for practical purposes

---

# 2. Depth First Search (DFS)

## Theory
**DFS** explores as far as possible along each branch before backtracking. Uses a stack (recursive calls or explicit stack).

## Key Applications
- **Traversal:** Visit all nodes
- **Cycle Detection:** In directed/undirected graphs
- **Path Finding:** Find any path between nodes
- **Topological Sort:** DFS-based approach
- **Connected Components:** Find all components

## Templates

### Recursive DFS (Adjacency List)
```cpp
class DFS {
    vector<vector<int>> adj;
    vector<bool> visited;
    vector<int> path;
    
public:
    void dfs(int node) {
        visited[node] = true;
        path.push_back(node);
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                dfs(neighbor);
            }
        }
    }
    
    // Complete traversal from starting node
    vector<int> dfsTraversal(vector<vector<int>>& graph, int start) {
        int n = graph.size();
        adj = graph;
        visited.assign(n, false);
        path.clear();
        
        dfs(start);
        return path;
    }
};
```

### Iterative DFS
```cpp
vector<int> dfsIterative(vector<vector<int>>& adj, int start) {
    int n = adj.size();
    vector<bool> visited(n, false);
    vector<int> path;
    stack<int> stk;
    
    stk.push(start);
    
    while (!stk.empty()) {
        int node = stk.top();
        stk.pop();
        
        if (!visited[node]) {
            visited[node] = true;
            path.push_back(node);
            
            // Add neighbors in reverse order for consistent traversal
            for (int i = adj[node].size() - 1; i >= 0; i--) {
                if (!visited[adj[node][i]]) {
                    stk.push(adj[node][i]);
                }
            }
        }
    }
    
    return path;
}
```

### Cycle Detection in Directed Graph
```cpp
class CycleDetection {
    vector<vector<int>> adj;
    vector<int> color; // 0: white, 1: gray, 2: black
    
    bool dfs(int node) {
        color[node] = 1; // Gray (visiting)
        
        for (int neighbor : adj[node]) {
            if (color[neighbor] == 1) return true; // Back edge = cycle
            if (color[neighbor] == 0 && dfs(neighbor)) return true;
        }
        
        color[node] = 2; // Black (visited)
        return false;
    }
    
public:
    bool hasCycle(vector<vector<int>>& graph) {
        int n = graph.size();
        adj = graph;
        color.assign(n, 0);
        
        for (int i = 0; i < n; i++) {
            if (color[i] == 0 && dfs(i)) {
                return true;
            }
        }
        return false;
    }
};
```

### DFS on 2D Grid
```cpp
class GridDFS {
    vector<vector<int>> directions = {{0,1}, {1,0}, {0,-1}, {-1,0}};
    int m, n;
    
    void dfs(vector<vector<int>>& grid, int x, int y, vector<vector<bool>>& visited) {
        if (x < 0 || x >= m || y < 0 || y >= n || visited[x][y] || grid[x][y] == 0) {
            return;
        }
        
        visited[x][y] = true;
        
        for (auto& dir : directions) {
            dfs(grid, x + dir[0], y + dir[1], visited);
        }
    }
    
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty()) return 0;
        m = grid.size(), n = grid[0].size();
        
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        int islands = 0;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1' && !visited[i][j]) {
                    dfs(grid, i, j, visited);
                    islands++;
                }
            }
        }
        
        return islands;
    }
};
```

## Example Problems
- **Number of Islands**
- **Clone Graph**
- **Path Sum in Binary Tree**
- **Course Schedule II** (Topological Sort)
- **Word Search**

## Time & Space Complexity
- **Time:** O(V + E) for adjacency list, O(V²) for adjacency matrix
- **Space:** O(V) for visited array + O(V) for recursion stack

---

# 3. Breadth First Search (BFS)

## Theory
**BFS** explores neighbors level by level using a queue. Guarantees shortest path in unweighted graphs.

## Key Applications
- **Shortest Path:** Unweighted graphs
- **Level-Order Traversal:** Trees
- **Multi-Source Problems:** Starting from multiple points
- **Minimum Steps:** Game-like problems

## Templates

### Basic BFS Template
```cpp
vector<int> bfs(vector<vector<int>>& adj, int start) {
    int n = adj.size();
    vector<bool> visited(n, false);
    vector<int> result;
    queue<int> q;
    
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        result.push_back(node);
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
    
    return result;
}
```

### Shortest Path BFS
```cpp
int shortestPath(vector<vector<int>>& adj, int start, int end) {
    int n = adj.size();
    vector<bool> visited(n, false);
    queue<pair<int, int>> q; // {node, distance}
    
    q.push({start, 0});
    visited[start] = true;
    
    while (!q.empty()) {
        auto [node, dist] = q.front();
        q.pop();
        
        if (node == end) return dist;
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push({neighbor, dist + 1});
            }
        }
    }
    
    return -1; // No path found
}
```

### Level-by-Level BFS
```cpp
vector<vector<int>> bfsLevels(vector<vector<int>>& adj, int start) {
    vector<vector<int>> levels;
    vector<bool> visited(adj.size(), false);
    queue<int> q;
    
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        int levelSize = q.size();
        vector<int> currentLevel;
        
        for (int i = 0; i < levelSize; i++) {
            int node = q.front();
            q.pop();
            currentLevel.push_back(node);
            
            for (int neighbor : adj[node]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.push(neighbor);
                }
            }
        }
        
        levels.push_back(currentLevel);
    }
    
    return levels;
}
```

### Multi-Source BFS Template
```cpp
vector<vector<int>> multiSourceBFS(vector<vector<int>>& grid, vector<pair<int,int>>& sources) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> dist(m, vector<int>(n, -1));
    vector<vector<int>> directions = {{0,1}, {1,0}, {0,-1}, {-1,0}};
    queue<pair<int,int>> q;
    
    // Initialize all sources
    for (auto [x, y] : sources) {
        q.push({x, y});
        dist[x][y] = 0;
    }
    
    while (!q.empty()) {
        auto [x, y] = q.front();
        q.pop();
        
        for (auto [dx, dy] : directions) {
            int nx = x + dx, ny = y + dy;
            
            if (nx >= 0 && nx < m && ny >= 0 && ny < n && 
                dist[nx][ny] == -1 && grid[nx][ny] != 0) {
                dist[nx][ny] = dist[x][y] + 1;
                q.push({nx, ny});
            }
        }
    }
    
    return dist;
}
```

## Example Problems
- **01 Matrix** (Multi-source BFS)
- **Rotting Oranges**
- **Word Ladder**
- **Binary Tree Level Order Traversal**
- **Knight Shortest Path**

## Time & Space Complexity
- **Time:** O(V + E) for adjacency list
- **Space:** O(V) for queue and visited array

---

# 4. Minimum Spanning Tree

## Theory
A **Minimum Spanning Tree (MST)** is a subgraph that connects all vertices with minimum total edge weight. Two main algorithms:

### Kruskal's Algorithm
- **Approach:** Sort edges by weight, use Union-Find to avoid cycles
- **Best for:** Sparse graphs, when edges are given as list

### Prim's Algorithm  
- **Approach:** Grow tree from starting vertex using priority queue
- **Best for:** Dense graphs, adjacency matrix representation

## Templates

### Kruskal's Algorithm
```cpp
struct Edge {
    int u, v, weight;
    bool operator<(const Edge& other) const {
        return weight < other.weight;
    }
};

class Kruskal {
    UnionFind uf;
    
public:
    vector<Edge> findMST(int n, vector<Edge>& edges) {
        uf = UnionFind(n);
        sort(edges.begin(), edges.end());
        
        vector<Edge> mst;
        int totalWeight = 0;
        
        for (const Edge& edge : edges) {
            if (uf.unite(edge.u, edge.v)) {
                mst.push_back(edge);
                totalWeight += edge.weight;
                
                if (mst.size() == n - 1) break; // MST complete
            }
        }
        
        return mst;
    }
};
```

### Prim's Algorithm
```cpp
class Prim {
public:
    vector<Edge> findMST(vector<vector<pair<int, int>>>& adj) {
        int n = adj.size();
        vector<bool> inMST(n, false);
        priority_queue<pair<int, pair<int, int>>, 
                      vector<pair<int, pair<int, int>>>, 
                      greater<>> pq; // {weight, {u, v}}
        
        vector<Edge> mst;
        int totalWeight = 0;
        
        // Start from vertex 0
        inMST[0] = true;
        for (auto [neighbor, weight] : adj[0]) {
            pq.push({weight, {0, neighbor}});
        }
        
        while (!pq.empty() && mst.size() < n - 1) {
            auto [weight, edge] = pq.top();
            pq.pop();
            auto [u, v] = edge;
            
            if (inMST[v]) continue; // Skip if already in MST
            
            // Add to MST
            mst.push_back({u, v, weight});
            totalWeight += weight;
            inMST[v] = true;
            
            // Add new edges from v
            for (auto [neighbor, w] : adj[v]) {
                if (!inMST[neighbor]) {
                    pq.push({w, {v, neighbor}});
                }
            }
        }
        
        return mst;
    }
};
```

### MST Applications Template
```cpp
class MSTApplications {
public:
    // Check if graph can form connected MST
    bool canFormMST(int n, vector<Edge>& edges) {
        UnionFind uf(n);
        sort(edges.begin(), edges.end());
        
        int edgesUsed = 0;
        for (const Edge& edge : edges) {
            if (uf.unite(edge.u, edge.v)) {
                edgesUsed++;
                if (edgesUsed == n - 1) return true;
            }
        }
        return false;
    }
    
    // Find minimum cost to connect all cities
    int minCostConnectPoints(vector<vector<int>>& points) {
        int n = points.size();
        vector<Edge> edges;
        
        // Generate all edges with Manhattan distance
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int dist = abs(points[i][0] - points[j][0]) + 
                          abs(points[i][1] - points[j][1]);
                edges.push_back({i, j, dist});
            }
        }
        
        // Apply Kruskal's algorithm
        UnionFind uf(n);
        sort(edges.begin(), edges.end());
        
        int totalCost = 0;
        for (const Edge& edge : edges) {
            if (uf.unite(edge.u, edge.v)) {
                totalCost += edge.weight;
            }
        }
        
        return totalCost;
    }
};
```

## Example Problems
- **Minimum Cost to Connect Sticks**
- **Connecting Cities with Minimum Cost**
- **Min Cost to Connect All Points**
- **Critical Connections in Network**

## Time Complexity
- **Kruskal's:** O(E log E) - dominated by sorting
- **Prim's:** O(E log V) with binary heap, O(E + V log V) with Fibonacci heap

---

# 5. Single Source Shortest Path

## Theory
Find shortest paths from one source to all other vertices. Main algorithms:

### Dijkstra's Algorithm
- **For:** Non-negative edge weights
- **Approach:** Greedy using priority queue
- **Time:** O((V + E) log V)

### Bellman-Ford Algorithm  
- **For:** Negative edge weights (detects negative cycles)
- **Approach:** Dynamic programming with relaxation
- **Time:** O(VE)

## Templates

### Dijkstra's Algorithm
```cpp
class Dijkstra {
public:
    vector<int> shortestPath(vector<vector<pair<int, int>>>& adj, int src) {
        int n = adj.size();
        vector<int> dist(n, INT_MAX);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        
        dist[src] = 0;
        pq.push({0, src}); // {distance, vertex}
        
        while (!pq.empty()) {
            auto [d, u] = pq.top();
            pq.pop();
            
            if (d > dist[u]) continue; // Skip outdated entry
            
            for (auto [v, weight] : adj[u]) {
                if (dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    pq.push({dist[v], v});
                }
            }
        }
        
        return dist;
    }
    
    // With path reconstruction
    pair<vector<int>, vector<int>> shortestPathWithPath(
        vector<vector<pair<int, int>>>& adj, int src, int dest) {
        
        int n = adj.size();
        vector<int> dist(n, INT_MAX), parent(n, -1);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        
        dist[src] = 0;
        pq.push({0, src});
        
        while (!pq.empty()) {
            auto [d, u] = pq.top();
            pq.pop();
            
            if (d > dist[u]) continue;
            
            for (auto [v, weight] : adj[u]) {
                if (dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    parent[v] = u;
                    pq.push({dist[v], v});
                }
            }
        }
        
        // Reconstruct path
        vector<int> path;
        for (int curr = dest; curr != -1; curr = parent[curr]) {
            path.push_back(curr);
        }
        reverse(path.begin(), path.end());
        
        return {dist, path};
    }
};
```

### Bellman-Ford Algorithm
```cpp
class BellmanFord {
public:
    pair<vector<int>, bool> shortestPath(int n, vector<vector<int>>& edges, int src) {
        vector<int> dist(n, INT_MAX);
        dist[src] = 0;
        
        // Relax edges n-1 times
        for (int i = 0; i < n - 1; i++) {
            for (auto& edge : edges) {
                int u = edge[0], v = edge[1], weight = edge[2];
                if (dist[u] != INT_MAX && dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                }
            }
        }
        
        // Check for negative cycles
        bool hasNegativeCycle = false;
        for (auto& edge : edges) {
            int u = edge[0], v = edge[1], weight = edge[2];
            if (dist[u] != INT_MAX && dist[u] + weight < dist[v]) {
                hasNegativeCycle = true;
                break;
            }
        }
        
        return {dist, hasNegativeCycle};
    }
};
```

### SPFA (Shortest Path Faster Algorithm)
```cpp
class SPFA {
public:
    vector<int> shortestPath(vector<vector<pair<int, int>>>& adj, int src) {
        int n = adj.size();
        vector<int> dist(n, INT_MAX);
        vector<bool> inQueue(n, false);
        queue<int> q;
        
        dist[src] = 0;
        q.push(src);
        inQueue[src] = true;
        
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            inQueue[u] = false;
            
            for (auto [v, weight] : adj[u]) {
                if (dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    if (!inQueue[v]) {
                        q.push(v);
                        inQueue[v] = true;
                    }
                }
            }
        }
        
        return dist;
    }
};
```

### A* Algorithm (Heuristic-based)
```cpp
class AStar {
    // Heuristic function (e.g., Manhattan distance for grid)
    int heuristic(int curr, int goal, vector<pair<int, int>>& coords) {
        return abs(coords[curr].first - coords[goal].first) + 
               abs(coords[curr].second - coords[goal].second);
    }
    
public:
    int shortestPath(vector<vector<pair<int, int>>>& adj, int src, int dest,
                    vector<pair<int, int>>& coordinates) {
        int n = adj.size();
        vector<int> dist(n, INT_MAX), f(n, INT_MAX);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        
        dist[src] = 0;
        f[src] = heuristic(src, dest, coordinates);
        pq.push({f[src], src});
        
        while (!pq.empty()) {
            auto [fScore, u] = pq.top();
            pq.pop();
            
            if (u == dest) return dist[u];
            if (fScore > f[u]) continue;
            
            for (auto [v, weight] : adj[u]) {
                int tentativeDist = dist[u] + weight;
                if (tentativeDist < dist[v]) {
                    dist[v] = tentativeDist;
                    f[v] = dist[v] + heuristic(v, dest, coordinates);
                    pq.push({f[v], v});
                }
            }
        }
        
        return -1;
    }
};
```

## Example Problems
- **Network Delay Time**
- **Cheapest Flights Within K Stops**
- **Path with Maximum Probability**
- **Minimum Effort Path**

## Time Complexity
- **Dijkstra:** O((V + E) log V)
- **Bellman-Ford:** O(VE)
- **SPFA:** O(VE) worst case, often much better in practice

---

# 6. Topological Sorting (Kahn's Algorithm)

## Theory
**Topological sorting** produces a linear ordering of vertices in a Directed Acyclic Graph (DAG) such that for every directed edge (u,v), vertex u comes before v.

### Kahn's Algorithm
- **Approach:** Remove vertices with in-degree 0 iteratively
- **Applications:** Course scheduling, build systems, dependency resolution

## Templates

### Kahn's Algorithm
```cpp
class TopologicalSort {
public:
    vector<int> kahnAlgorithm(vector<vector<int>>& adj) {
        int n = adj.size();
        vector<int> indegree(n, 0);
        
        // Calculate in-degrees
        for (int u = 0; u < n; u++) {
            for (int v : adj[u]) {
                indegree[v]++;
            }
        }
        
        // Initialize queue with vertices having in-degree 0
        queue<int> q;
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }
        
        vector<int> result;
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            result.push_back(u);
            
            // Reduce in-degree of neighbors
            for (int v : adj[u]) {
                indegree[v]--;
                if (indegree[v] == 0) {
                    q.push(v);
                }
            }
        }
        
        // Check for cycle (if result.size() < n, there's a cycle)
        return result.size() == n ? result : vector<int>();
    }
    
    // Check if topological sort exists (no cycle)
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adj(numCourses);
        vector<int> indegree(numCourses, 0);
        
        // Build graph
        for (auto& pre : prerequisites) {
            adj[pre[1]].push_back(pre[0]); // pre[1] -> pre[0]
            indegree[pre[0]]++;
        }
        
        queue<int> q;
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }
        
        int processed = 0;
        while (!q.empty()) {
            int course = q.front();
            q.pop();
            processed++;
            
            for (int nextCourse : adj[course]) {
                indegree[nextCourse]--;
                if (indegree[nextCourse] == 0) {
                    q.push(nextCourse);
                }
            }
        }
        
        return processed == numCourses;
    }
};
```

### DFS-based Topological Sort
```cpp
class TopologicalSortDFS {
    vector<vector<int>> adj;
    vector<bool> visited;
    vector<int> result;
    
    void dfs(int node) {
        visited[node] = true;
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                dfs(neighbor);
            }
        }
        
        result.push_back(node); // Add to result after visiting all neighbors
    }
    
public:
    vector<int> topologicalSort(vector<vector<int>>& graph) {
        int n = graph.size();
        adj = graph;
        visited.assign(n, false);
        result.clear();
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i);
            }
        }
        
        reverse(result.begin(), result.end()); // Reverse for correct order
        return result;
    }
};
```

### Advanced Applications
```cpp
class TopologicalApplications {
public:
    // Find order to finish all courses
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adj(numCourses);
        vector<int> indegree(numCourses, 0);
        
        for (auto& pre : prerequisites) {
            adj[pre[1]].push_back(pre[0]);
            indegree[pre[0]]++;
        }
        
        queue<int> q;
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }
        
        vector<int> order;
        while (!q.empty()) {
            int course = q.front();
            q.pop();
            order.push_back(course);
            
            for (int nextCourse : adj[course]) {
                indegree[nextCourse]--;
                if (indegree[nextCourse] == 0) {
                    q.push(nextCourse);
                }
            }
        }
        
        return order.size() == numCourses ? order : vector<int>();
    }
    
    // Minimum height trees
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if (n == 1) return {0};
        
        vector<set<int>> adj(n);
        for (auto& edge : edges) {
            adj[edge[0]].insert(edge[1]);
            adj[edge[1]].insert(edge[0]);
        }
        
        queue<int> leaves;
        for (int i = 0; i < n; i++) {
            if (adj[i].size() == 1) {
                leaves.push(i);
            }
        }
        
        int remaining = n;
        while (remaining > 2) {
            int leafCount = leaves.size();
            remaining -= leafCount;
            
            for (int i = 0; i < leafCount; i++) {
                int leaf = leaves.front();
                leaves.pop();
                
                int neighbor = *adj[leaf].begin();
                adj[neighbor].erase(leaf);
                
                if (adj[neighbor].size() == 1) {
                    leaves.push(neighbor);
                }
            }
        }
        
        vector<int> result;
        while (!leaves.empty()) {
            result.push_back(leaves.front());
            leaves.pop();
        }
        return result;
    }
};
```

## Example Problems
- **Course Schedule I & II**
- **Alien Dictionary**
- **Minimum Height Trees**
- **Parallel Courses**

## Time Complexity
- **Kahn's Algorithm:** O(V + E)
- **DFS-based:** O(V + E)
- **Space:** O(V) for in-degree array and queue

---

# Summary & Quick Reference

## Algorithm Selection Guide

| Problem Type | Algorithm | Time Complexity | Use When |
|--------------|-----------|----------------|----------|
| **Connected Components** | Union-Find | O(α(n)) per op | Dynamic connectivity |
| **Any Path** | DFS | O(V + E) | Explore all possibilities |
| **Shortest Path (Unweighted)** | BFS | O(V + E) | Minimum steps/distance |
| **Shortest Path (Weighted)** | Dijkstra | O((V+E)log V) | Non-negative weights |
| **Shortest Path (Negative)** | Bellman-Ford | O(VE) | Negative edges |
| **Minimum Spanning Tree** | Kruskal/Prim | O(E log E) | Connect all with min cost |
| **Dependency Resolution** | Topological Sort | O(V + E) | Ordering with constraints |

## Common Patterns

### Graph Representation
```cpp
// Adjacency List (most common)
vector<vector<int>> adj(n);

// Adjacency Matrix (dense graphs)
vector<vector<int>> matrix(n, vector<int>(n, 0));

// Edge List (for algorithms like Kruskal's)
vector<vector<int>> edges; // {u, v, weight}
```

### Standard Checks
```cpp
// Boundary check for grids
bool valid(int x, int y, int m, int n) {
    return x >= 0 && x < m && y >= 0 && y < n;
}

// Cycle detection
bool hasCycle = (processedNodes < totalNodes);
```

### Optimization Tips
1. **Use appropriate data structures** (priority_queue for Dijkstra, UnionFind for connectivity)
2. **Path compression and union by rank** for UnionFind
3. **Early termination** when target is found
4. **Bidirectional search** for shortest path between two specific nodes
5. **Coordinate compression** for large coordinate spaces

This guide covers the fundamental graph algorithms with templates ready for competitive programming and technical interviews!
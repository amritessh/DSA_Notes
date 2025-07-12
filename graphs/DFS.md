In Graph theory, the depth-first search algorithm (abbreviated as DFS) is mainly used to:

Traverse all vertices in a “graph”;
Traverse all paths between any two vertices in a “graph”.

When we traverse an adjacent vertex, we completely finish the traversal of all vertices reachable through that adjacent vertex.

DFS - Stack

<<<<<<< Updated upstream
# 🎯 Complete DFS Templates: Recursive & Iterative

## 🌟 Universal DFS Templates (MEMORIZE THESE!)

### 🔥 Recursive DFS Template
```cpp
bool dfs(GRAPH_TYPE& graph, int node, VISITED_TYPE& visited, PARAMS...) {
    // 1️⃣ BASE CASES
    if (OUT_OF_BOUNDS_CHECK) return false;
    if (visited[node] || INVALID_CONDITION) return false;
    if (TARGET_REACHED) return true;
    
    // 2️⃣ MARK VISITED
    visited[node] = true;
    
    // 3️⃣ PROCESS CURRENT NODE
    // Do something with current node
    
    // 4️⃣ EXPLORE NEIGHBORS
    for (int neighbor : getNeighbors(node)) {
        if (dfs(graph, neighbor, visited, params...)) {
            return true;  // Early return if solution found
        }
    }
    
    // 5️⃣ BACKTRACK (if needed)
    visited[node] = false;  // Only for backtracking problems
    
    return false;
}
```

### 🔥 Iterative DFS Template
```cpp
bool dfsIterative(GRAPH_TYPE& graph, int start, VISITED_TYPE& visited, PARAMS...) {
    stack<int> stk;
    stk.push(start);
    
    while (!stk.empty()) {
        int node = stk.top();
        stk.pop();
        
        // 1️⃣ SKIP IF ALREADY VISITED
        if (visited[node]) continue;
        
        // 2️⃣ BASE CASES
        if (OUT_OF_BOUNDS_CHECK) continue;
        if (INVALID_CONDITION) continue;
        if (TARGET_REACHED) return true;
        
        // 3️⃣ MARK VISITED
        visited[node] = true;
        
        // 4️⃣ PROCESS CURRENT NODE
        // Do something with current node
        
        // 5️⃣ ADD NEIGHBORS TO STACK
        for (int neighbor : getNeighbors(node)) {
            if (!visited[neighbor]) {
                stk.push(neighbor);
            }
        }
    }
    
    return false;
}
```

## 🎨 Concrete Examples

### 1️⃣ Number of Islands

#### Recursive Version
```cpp
class IslandsRecursive {
private:
    vector<vector<int>> directions = {{-1,0}, {1,0}, {0,-1}, {0,1}};
    int rows, cols;
    
    void dfs(vector<vector<char>>& grid, int r, int c) {
        // Base cases
        if (r < 0 || r >= rows || c < 0 || c >= cols) return;
        if (grid[r][c] == '0') return;
        
        // Mark visited by changing to '0'
        grid[r][c] = '0';
        
        // Explore 4 directions
        for (auto& dir : directions) {
            dfs(grid, r + dir[0], c + dir[1]);
        }
    }
    
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty()) return 0;
        
        rows = grid.size();
        cols = grid[0].size();
        int count = 0;
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    count++;
                }
            }
        }
        
        return count;
    }
};
```

#### Iterative Version
```cpp
class IslandsIterative {
private:
    vector<vector<int>> directions = {{-1,0}, {1,0}, {0,-1}, {0,1}};
    int rows, cols;
    
    void dfsIterative(vector<vector<char>>& grid, int startR, int startC) {
        stack<pair<int, int>> stk;
        stk.push({startR, startC});
        
        while (!stk.empty()) {
            auto [r, c] = stk.top();
            stk.pop();
            
            // Base cases
            if (r < 0 || r >= rows || c < 0 || c >= cols) continue;
            if (grid[r][c] == '0') continue;
            
            // Mark visited
            grid[r][c] = '0';
            
            // Add neighbors to stack
            for (auto& dir : directions) {
                stk.push({r + dir[0], c + dir[1]});
            }
        }
    }
    
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty()) return 0;
        
        rows = grid.size();
        cols = grid[0].size();
        int count = 0;
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == '1') {
                    dfsIterative(grid, i, j);
                    count++;
                }
            }
        }
        
        return count;
    }
};
```

### 2️⃣ Path Exists

#### Recursive Version
```cpp
class PathExistsRecursive {
private:
    bool dfs(vector<vector<int>>& adj, int node, int target, vector<bool>& visited) {
        if (node == target) return true;
        
        visited[node] = true;
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                if (dfs(adj, neighbor, target, visited)) {
                    return true;
                }
            }
        }
        
        return false;
    }
    
public:
    bool hasPath(vector<vector<int>>& adj, int start, int target) {
        vector<bool> visited(adj.size(), false);
        return dfs(adj, start, target, visited);
    }
};
```

#### Iterative Version
```cpp
class PathExistsIterative {
public:
    bool hasPath(vector<vector<int>>& adj, int start, int target) {
        vector<bool> visited(adj.size(), false);
        stack<int> stk;
        stk.push(start);
        
        while (!stk.empty()) {
            int node = stk.top();
            stk.pop();
            
            if (visited[node]) continue;
            if (node == target) return true;
            
            visited[node] = true;
            
            for (int neighbor : adj[node]) {
                if (!visited[neighbor]) {
                    stk.push(neighbor);
                }
            }
        }
        
        return false;
    }
};
```

### 3️⃣ Cycle Detection

#### Recursive Version
```cpp
class CycleDetectionRecursive {
private:
    bool dfs(vector<vector<int>>& adj, int node, vector<bool>& visited, vector<bool>& recStack) {
        visited[node] = true;
        recStack[node] = true;
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                if (dfs(adj, neighbor, visited, recStack)) {
                    return true;
                }
            } else if (recStack[neighbor]) {
                return true;  // Back edge = cycle
            }
        }
        
        recStack[node] = false;
        return false;
    }
    
public:
    bool hasCycle(vector<vector<int>>& adj) {
        int n = adj.size();
        vector<bool> visited(n, false);
        vector<bool> recStack(n, false);
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                if (dfs(adj, i, visited, recStack)) {
                    return true;
                }
            }
        }
        
        return false;
    }
};
```

#### Iterative Version (Undirected Graph)
```cpp
class CycleDetectionIterative {
public:
    bool hasCycle(vector<vector<int>>& adj) {
        int n = adj.size();
        vector<bool> visited(n, false);
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                stack<pair<int, int>> stk;  // {node, parent}
                stk.push({i, -1});
                
                while (!stk.empty()) {
                    auto [node, parent] = stk.top();
                    stk.pop();
                    
                    if (visited[node]) continue;
                    visited[node] = true;
                    
                    for (int neighbor : adj[node]) {
                        if (!visited[neighbor]) {
                            stk.push({neighbor, node});
                        } else if (neighbor != parent) {
                            return true;  // Cycle found
                        }
                    }
                }
            }
        }
        
        return false;
    }
};
```

### 4️⃣ Find Path

#### Recursive Version
```cpp
class FindPathRecursive {
private:
    bool dfs(vector<vector<int>>& adj, int node, int target, vector<bool>& visited, vector<int>& path) {
        visited[node] = true;
        path.push_back(node);
        
        if (node == target) {
            return true;
        }
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                if (dfs(adj, neighbor, target, visited, path)) {
                    return true;
                }
            }
        }
        
        path.pop_back();  // Backtrack
        return false;
    }
    
public:
    vector<int> findPath(vector<vector<int>>& adj, int start, int target) {
        vector<bool> visited(adj.size(), false);
        vector<int> path;
        
        if (dfs(adj, start, target, visited, path)) {
            return path;
        }
        
        return {};
    }
};
```

#### Iterative Version
```cpp
class FindPathIterative {
public:
    vector<int> findPath(vector<vector<int>>& adj, int start, int target) {
        vector<bool> visited(adj.size(), false);
        stack<pair<int, vector<int>>> stk;  // {node, path}
        stk.push({start, {start}});
        
        while (!stk.empty()) {
            auto [node, path] = stk.top();
            stk.pop();
            
            if (visited[node]) continue;
            if (node == target) return path;
            
            visited[node] = true;
            
            for (int neighbor : adj[node]) {
                if (!visited[neighbor]) {
                    vector<int> newPath = path;
                    newPath.push_back(neighbor);
                    stk.push({neighbor, newPath});
                }
            }
        }
        
        return {};
    }
};
```

### 5️⃣ Connected Components

#### Recursive Version
```cpp
class ComponentsRecursive {
private:
    void dfs(vector<vector<int>>& adj, int node, vector<bool>& visited) {
        visited[node] = true;
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                dfs(adj, neighbor, visited);
            }
        }
    }
    
public:
    int countComponents(vector<vector<int>>& adj) {
        int n = adj.size();
        vector<bool> visited(n, false);
        int components = 0;
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(adj, i, visited);
                components++;
            }
        }
        
        return components;
    }
};
```

#### Iterative Version
```cpp
class ComponentsIterative {
public:
    int countComponents(vector<vector<int>>& adj) {
        int n = adj.size();
        vector<bool> visited(n, false);
        int components = 0;
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                stack<int> stk;
                stk.push(i);
                
                while (!stk.empty()) {
                    int node = stk.top();
                    stk.pop();
                    
                    if (visited[node]) continue;
                    visited[node] = true;
                    
                    for (int neighbor : adj[node]) {
                        if (!visited[neighbor]) {
                            stk.push(neighbor);
                        }
                    }
                }
                
                components++;
            }
        }
        
        return components;
    }
};
```

## 🎯 When to Use Which?

### 🌟 Use Recursive DFS When:
- **Tree problems** (binary tree, N-ary tree)
- **Small to medium graphs** (< 10,000 nodes)
- **Backtracking required** (path finding, combinations)
- **Clean, readable code preferred**
- **Interview situations** (easier to explain)

### 🌟 Use Iterative DFS When:
- **Very deep graphs** (avoid stack overflow)
- **Large graphs** (> 10,000 nodes)
- **Production systems** (more control)
- **Memory constraints**
- **Performance critical code**

## 🧠 5-Step Memorization Checklist

### For Any DFS Problem:
1. **🚫 What are my base cases?** (bounds, visited, target)
2. **✅ How do I mark visited?** (boolean array, modify input)
3. **🔄 What processing do I need?** (count, collect, modify)
4. **👥 Who are the neighbors?** (4-directions, adjacency list)
5. **🔙 Do I need backtracking?** (path problems, combinations)

## 🎯 Problem Recognition Patterns

**When you see these keywords → Think DFS:**
- "Connected components"
- "Islands" or "Regions"
- "Path exists"
- "Cycle detection"
- "Topological order"
- "Explore all possibilities"
- "Backtracking"

## 🔧 Quick Adaptations

### For Grid Problems:
```cpp
// 4-directional neighbors
vector<vector<int>> directions = {{-1,0}, {1,0}, {0,-1}, {0,1}};

// Boundary check
if (r < 0 || r >= rows || c < 0 || c >= cols) return;

// Explore neighbors
for (auto& dir : directions) {
    dfs(grid, r + dir[0], c + dir[1], visited);
}
```

### For Adjacency List:
```cpp
// Neighbors from adjacency list
for (int neighbor : adj[node]) {
    if (!visited[neighbor]) {
        dfs(adj, neighbor, visited);
    }
}
```

### For Cycle Detection:
```cpp
// Need recursion stack for directed graphs
vector<bool> recStack(n, false);
recStack[node] = true;   // Enter recursion
// ... dfs calls ...
recStack[node] = false;  // Exit recursion
```

## 🏆 The Ultimate DFS Strategy

### Step 1: Pattern Recognition
- Identify if it's a DFS problem
- Determine graph type (grid, adjacency list, etc.)

### Step 2: Choose Template
- Recursive for simplicity
- Iterative for deep graphs

### Step 3: Fill the Template
- Copy the appropriate template
- Fill in problem-specific details
- Adjust parameters and return types

### Step 4: Test & Debug
- Test with provided examples
- Check edge cases
- Verify base cases

## 🎯 Complexity Analysis

| Operation | Adjacency List | Adjacency Matrix |
|-----------|---------------|------------------|
| **Time** | O(V + E) | O(V²) |
| **Space** | O(V) | O(V) |

**Where:**
- V = number of vertices
- E = number of edges

## 🚀 Master These Templates!

Practice these templates on:
1. **LeetCode 200** - Number of Islands
2. **LeetCode 547** - Friend Circles
3. **LeetCode 207** - Course Schedule
4. **LeetCode 417** - Pacific Atlantic Water Flow
5. **LeetCode 130** - Surrounded Regions

**Once you memorize these patterns, you'll solve 95% of DFS problems effortlessly!** 🌟
=======
The depth-first search algorithm allows us to determine whether two nodes, node x and node y, have a path between them. The DFS algorithm does this by looking at all of the children of the starting node, node x, until it reaches node y. It does this by recursively taking the same steps, again and again, in order to determine if such a path between two nodes even exists.


two major points to keep in mind when initiating a graph traversal: first, we can choose any arbitrary node to start our traversal with, since there is no concept of a “root” nodes the way that there are in tree structures. And second, whatever we do, we want to ensure that we don’t repeat any nodes; that is to say, once we “visit” a node, we don’t want to visit it again.
>>>>>>> Stashed changes

# 🎯 Same Data Type, Different Meaning: Edge List vs Adjacency List

## 🚨 The Confusion

**You're absolutely right to be confused!** Both use `vector<vector<int>>`, but the **CONTENT/MEANING** is completely different.

## 📊 Side-by-Side Comparison

### **SAME Graph:**
```
    0 ─── 1
    │     │
    │     │
    2 ─── 3
```

### **🔥 Edge List Representation:**
```cpp
vector<vector<int>> edges = [
    [0, 1],    // Edge between vertex 0 and vertex 1
    [0, 2],    // Edge between vertex 0 and vertex 2
    [1, 3],    // Edge between vertex 1 and vertex 3
    [2, 3]     // Edge between vertex 2 and vertex 3
];
```

**What each row means:**
- `edges[0] = [0, 1]` → "There's an edge from 0 to 1"
- `edges[1] = [0, 2]` → "There's an edge from 0 to 2"
- `edges[2] = [1, 3]` → "There's an edge from 1 to 3"
- `edges[3] = [2, 3]` → "There's an edge from 2 to 3"

### **🔥 Adjacency List Representation:**
```cpp
vector<vector<int>> adj = [
    [1, 2],    // Vertex 0 connects to vertices 1 and 2
    [0, 3],    // Vertex 1 connects to vertices 0 and 3
    [0, 3],    // Vertex 2 connects to vertices 0 and 3
    [1, 2]     // Vertex 3 connects to vertices 1 and 2
];
```

**What each row means:**
- `adj[0] = [1, 2]` → "Vertex 0's neighbors are 1 and 2"
- `adj[1] = [0, 3]` → "Vertex 1's neighbors are 0 and 3"
- `adj[2] = [0, 3]` → "Vertex 2's neighbors are 0 and 3"
- `adj[3] = [1, 2]` → "Vertex 3's neighbors are 1 and 2"

## 🎭 Key Differences

| Aspect | Edge List | Adjacency List |
|--------|-----------|---------------|
| **Data Type** | `vector<vector<int>>` | `vector<vector<int>>` |
| **Row Meaning** | Each row = 1 edge | Each row = 1 vertex |
| **Row Content** | `[u, v]` = edge from u to v | `[n1, n2, ...]` = neighbors |
| **Size** | Number of edges | Number of vertices |
| **Access Pattern** | Iterate through all edges | Direct vertex access |

## 🔍 Visual Breakdown

### **Edge List:**
```
Index | Content | Meaning
------|---------|----------
  0   | [0, 1]  | Edge: 0 ↔ 1
  1   | [0, 2]  | Edge: 0 ↔ 2
  2   | [1, 3]  | Edge: 1 ↔ 3
  3   | [2, 3]  | Edge: 2 ↔ 3
```

### **Adjacency List:**
```
Index | Content | Meaning
------|---------|----------
  0   | [1, 2]  | Vertex 0 → neighbors 1, 2
  1   | [0, 3]  | Vertex 1 → neighbors 0, 3
  2   | [0, 3]  | Vertex 2 → neighbors 0, 3
  3   | [1, 2]  | Vertex 3 → neighbors 1, 2
```

## 🎯 How to Tell the Difference

### **1. Look at the Variable Name:**
```cpp
vector<vector<int>>& edges     // Usually edge list
vector<vector<int>>& adj       // Usually adjacency list
vector<vector<int>>& graph     // Usually adjacency list
```

### **2. Look at the Function Parameters:**
```cpp
bool validPath(int n, vector<vector<int>>& edges, int source, int destination)
//                    ^^^^^^^^^^^^^^^^^^^^^ 
// Parameter name "edges" suggests edge list
```

### **3. Look at the Usage:**
```cpp
// Edge list usage:
for (auto& edge : edges) {
    int u = edge[0], v = edge[1];  // u and v are vertices
    // Process edge between u and v
}

// Adjacency list usage:
for (int i = 0; i < adj.size(); i++) {
    for (int neighbor : adj[i]) {  // i is vertex, neighbor is its neighbor
        // Process neighbor of vertex i
    }
}
```

## 🚀 Real Code Examples

### **Edge List Example (LeetCode style):**
```cpp
bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
    // edges is an edge list!
    // edges = [[0,1], [1,2], [2,0]]
    
    // Need to convert to adjacency list for efficient DFS
    vector<vector<int>> adj(n);
    for (auto& edge : edges) {
        adj[edge[0]].push_back(edge[1]);
        adj[edge[1]].push_back(edge[0]);
    }
    
    // Now use adjacency list for DFS
    return dfs(adj, source, destination);
}
```

### **Adjacency List Example:**
```cpp
bool hasPath(vector<vector<int>>& adj, int source, int destination) {
    // adj is already an adjacency list!
    // adj[0] = [1,2], adj[1] = [0,3], adj[2] = [0,3], adj[3] = [1,2]
    
    // Can directly use for DFS
    return dfs(adj, source, destination);
}
```

## 🎭 Conversion Example

### **Given Edge List:**
```cpp
vector<vector<int>> edges = [[0,1], [1,2], [2,0]];
```

### **Convert to Adjacency List:**
```cpp
int n = 3;  // Number of vertices
vector<vector<int>> adj(n);

for (auto& edge : edges) {
    int u = edge[0], v = edge[1];
    adj[u].push_back(v);
    adj[v].push_back(u);
}

// Result: adj = [[1,2], [0,2], [0,1]]
```

## 🧠 Memory Layout

### **Edge List:**
```
edges[0] → [0, 1]
edges[1] → [0, 2]  
edges[2] → [1, 3]
edges[3] → [2, 3]
```

### **Adjacency List:**
```
adj[0] → [1, 2]
adj[1] → [0, 3]
adj[2] → [0, 3]
adj[3] → [1, 2]
```

## 🎯 The "Aha!" Moment

**Same container (`vector<vector<int>>`), completely different meaning:**

- **Edge List**: Each inner vector is an **edge** (2 vertices)
- **Adjacency List**: Each inner vector is a **vertex's neighbors** (multiple vertices)

**It's like the difference between:**
- A **phone book** (edge list) → `["Alice-Bob", "Bob-Charlie", "Charlie-Alice"]`
- A **contact list** (adjacency list) → `["Alice: [Bob, Charlie]", "Bob: [Alice, Charlie]", "Charlie: [Alice, Bob]"]`

## 🚀 Quick Recognition Tips

1. **Size matters:**
   - Edge list: size = number of edges
   - Adjacency list: size = number of vertices

2. **Inner vector size:**
   - Edge list: usually size 2 (u, v)
   - Adjacency list: variable size (number of neighbors)

3. **Parameter name:**
   - `edges` → edge list
   - `adj`/`graph` → adjacency list

4. **Problem context:**
   - "Given edges..." → edge list
   - "Given adjacency list..." → adjacency list

**Now you know why we need the conversion!** 🌟

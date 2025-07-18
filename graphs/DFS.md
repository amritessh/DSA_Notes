In Graph theory, the depth-first search algorithm (abbreviated as DFS) is mainly used to:

Traverse all vertices in a “graph”;
Traverse all paths between any two vertices in a “graph”.

When we traverse an adjacent vertex, we completely finish the traversal of all vertices reachable through that adjacent vertex.

DFS - Stack

<<<<<<< Updated upstream
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
=======
The depth-first search algorithm allows us to determine whether two nodes, node x and node y, have a path between them. The DFS algorithm does this by looking at all of the children of the starting node, node x, until it reaches node y. It does this by recursively taking the same steps, again and again, in order to determine if such a path between two nodes even exists.


two major points to keep in mind when initiating a graph traversal: first, we can choose any arbitrary node to start our traversal with, since there is no concept of a “root” nodes the way that there are in tree structures. And second, whatever we do, we want to ensure that we don’t repeat any nodes; that is to say, once we “visit” a node, we don’t want to visit it again.
>>>>>>> Stashed changes

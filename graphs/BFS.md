# BFS Graph Templates - Complete Guide

## Table of Contents
1. [Key Concepts](#key-concepts)
2. [Template Selection Guide](#template-selection-guide)
3. [BFS Templates](#bfs-templates)
   - [1. Basic BFS Traversal](#1-basic-bfs-traversal)
   - [2. Shortest Path BFS](#2-shortest-path-bfs)
   - [3. Level-by-Level BFS](#3-level-by-level-bfs)
   - [4. BFS with Path Reconstruction](#4-bfs-with-path-reconstruction)
   - [5. BFS on 2D Grid (4-directional)](#5-bfs-on-2d-grid-4-directional)
   - [6. BFS on 2D Grid (8-directional)](#6-bfs-on-2d-grid-8-directional)
   - [7. Multi-Source BFS](#7-multi-source-bfs)
   - [8. BFS with State Tracking](#8-bfs-with-state-tracking)
4. [Common Mistakes & Debugging](#common-mistakes--debugging)
5. [Complexity Analysis](#complexity-analysis)
6. [Practice Problems](#practice-problems)

---

## Key Concepts

### When to Use BFS
✅ **Use BFS for:**
- Shortest path in **unweighted graphs**
- Level-order traversal
- Finding minimum steps/moves
- Connected components
- Spreading/propagation problems

❌ **Don't use BFS for:**
- Weighted graphs (use Dijkstra's instead)
- Finding ALL paths (use DFS)
- Deep recursion problems (use DFS)

### Core Properties
- **Explores nodes level by level** (breadth-first)
- Uses a **QUEUE** data structure (FIFO)
- **Guarantees shortest path** in unweighted graphs
- **Time:** O(V + E), **Space:** O(V)

---

## Template Selection Guide

| Problem Type | Keywords | Template |
|--------------|----------|----------|
| **Basic Traversal** | "Visit all nodes", "Check connectivity" | Template 1 |
| **Shortest Path** | "Minimum steps", "Shortest distance" | Template 2 |
| **Level Processing** | "Level order", "Distance k", "Depth" | Template 3 |
| **Path Reconstruction** | "Show route", "Print path", "Sequence" | Template 4 |
| **Grid Problems** | "Matrix", "2D array", "Maze", "Robot" | Template 5/6 |
| **Spreading Problems** | "Multiple sources", "Fire spread", "Infection" | Template 7 |
| **Complex State** | "Keys/doors", "Different conditions" | Template 8 |

---

## BFS Templates

### 1. Basic BFS Traversal

**Purpose:** Visit all reachable nodes from a starting point

**Use Cases:**
- Print all connected nodes
- Check if graph is connected
- Count connected components

**Key Points:**
- Mark nodes as visited **BEFORE** adding to queue (prevents duplicates)
- Process node when you **dequeue** it (not when you enqueue)

```cpp
void bfsTraversal(vector<vector<int>>& adj, int start) {
    int n = adj.size();
    vector<bool> visited(n, false);  // Track visited nodes
    queue<int> q;                    // BFS queue
    
    // Initialize: add starting node
    q.push(start);
    visited[start] = true;  // IMPORTANT: Mark as visited when adding to queue
    
    while (!q.empty()) {
        int node = q.front();  // Get front element
        q.pop();               // Remove from queue
        
        cout << node << " ";   // Process current node
        
        // Explore all neighbors
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {           // Only visit unvisited nodes
                visited[neighbor] = true;       // Mark as visited
                q.push(neighbor);               // Add to queue
            }
        }
    }
}
```

---

### 2. Shortest Path BFS

**Purpose:** Find shortest distance between two nodes in unweighted graph

**Use Cases:**
- Word Ladder problem
- Minimum moves in games (Chess Knight)
- Any "minimum steps" problem

**Why BFS Works for Shortest Path:**
- BFS explores nodes level by level
- First time we reach target = shortest path
- All edges have equal weight (1)

```cpp
int shortestPath(vector<vector<int>>& adj, int start, int target) {
    int n = adj.size();
    vector<bool> visited(n, false);
    queue<int> q;
    int distance = 0;  // Current level/distance from start
    
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        int size = q.size();  // Number of nodes at current level
        
        // Process ALL nodes at current level before moving to next level
        for (int i = 0; i < size; i++) {
            int node = q.front();
            q.pop();
            
            // Check if we reached target
            if (node == target) {
                return distance;  // Found shortest path!
            }
            
            // Add all unvisited neighbors (they'll be at distance+1)
            for (int neighbor : adj[node]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.push(neighbor);
                }
            }
        }
        distance++;  // Increment distance for next level
    }
    
    return -1; // Target not reachable
}
```

---

### 3. Level-by-Level BFS

**Purpose:** Group nodes by their distance from start node

**Use Cases:**
- Binary tree level order traversal
- Social network: friends at distance 1, 2, 3...
- N-ary tree problems

**Pattern Recognition:** If problem asks for "level", "depth", "generation" → use this template

```cpp
vector<vector<int>> bfsLevels(vector<vector<int>>& adj, int start) {
    int n = adj.size();
    vector<bool> visited(n, false);
    queue<int> q;
    vector<vector<int>> levels;  // Store nodes at each level
    
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        int size = q.size();         // Nodes at current level
        vector<int> currentLevel;    // Store current level nodes
        
        // Process ALL nodes at current level
        for (int i = 0; i < size; i++) {
            int node = q.front();
            q.pop();
            currentLevel.push_back(node);  // Add to current level
            
            // Add neighbors for NEXT level
            for (int neighbor : adj[node]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.push(neighbor);
                }
            }
        }
        
        levels.push_back(currentLevel);  // Store completed level
    }
    
    return levels;
}
```

**Example Output:**
```
Tree:    1
       / | \
      2  3  4
     /      |
    5       6

Result: [[1], [2,3,4], [5,6]]
```

---

### 4. BFS with Path Reconstruction

**Purpose:** Find shortest path AND reconstruct the actual path

**Use Cases:**
- Navigation systems (show route)
- Word ladder (show transformation sequence)
- Puzzle solving (show solution steps)

**Key Concept - Parent Tracking:**
- For each node, remember which node we came from
- `parent[child] = current_node`
- Trace back from target to start using parent array

```cpp
vector<int> bfsPath(vector<vector<int>>& adj, int start, int target) {
    int n = adj.size();
    vector<bool> visited(n, false);
    vector<int> parent(n, -1);  // parent[i] = node that led to node i
    queue<int> q;
    
    q.push(start);
    visited[start] = true;
    // start has no parent, so parent[start] = -1
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        // Found target! Now reconstruct path
        if (node == target) {
            vector<int> path;
            int current = target;
            
            // Trace back from target to start using parent pointers
            while (current != -1) {
                path.push_back(current);
                current = parent[current];  // Move to parent
            }
            
            // Path is built backwards, so reverse it
            reverse(path.begin(), path.end());
            return path;  // Returns [start, ..., target]
        }
        
        // Explore neighbors and set parent relationships
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                parent[neighbor] = node;  // Record how we reached neighbor
                q.push(neighbor);
            }
        }
    }
    
    return {};  // Empty vector = no path found
}
```

**Example - Path Reconstruction:**
```
Graph: 0-1-2-4
Finding path from 0 to 4:

Step 1: Visit 0, parent[0] = -1
Step 2: Visit 1, parent[1] = 0  
Step 3: Visit 2, parent[2] = 1
Step 4: Visit 4, parent[4] = 2, FOUND TARGET!

Reconstruction:
current = 4, path = [4]
current = parent[4] = 2, path = [4, 2]  
current = parent[2] = 1, path = [4, 2, 1]
current = parent[1] = 0, path = [4, 2, 1, 0]
current = parent[0] = -1, stop

Reverse: [0, 1, 2, 4] ✓
```

---

### 5. BFS on 2D Grid (4-directional)

**Purpose:** BFS on 2D matrix/grid problems

**Use Cases:**
- Shortest path in maze
- Island problems
- Flood fill algorithms
- Robot movement problems

**4-Directional Movement:**
- Up: (-1, 0)
- Down: (1, 0)
- Left: (0, -1)
- Right: (0, 1)

```cpp
int bfsGrid(vector<vector<int>>& grid, pair<int,int> start, pair<int,int> target) {
    int m = grid.size(), n = grid[0].size();  // m = rows, n = columns
    vector<vector<bool>> visited(m, vector<bool>(n, false));
    queue<pair<int,int>> q;
    
    // 4 directions: up, down, left, right
    vector<pair<int,int>> directions = {{-1,0}, {1,0}, {0,-1}, {0,1}};
    
    q.push(start);
    visited[start.first][start.second] = true;
    int steps = 0;
    
    while (!q.empty()) {
        int size = q.size();  // Level-by-level processing
        
        for (int i = 0; i < size; i++) {
            auto [x, y] = q.front();
            q.pop();
            
            // Check if reached target
            if (make_pair(x, y) == target) {
                return steps;
            }
            
            // Explore 4 directions
            for (auto [dx, dy] : directions) {
                int nx = x + dx, ny = y + dy;  // New coordinates
                
                // CRITICAL: Check all conditions before accessing grid
                if (nx >= 0 && nx < m &&           // Valid row
                    ny >= 0 && ny < n &&           // Valid column  
                    !visited[nx][ny] &&            // Not visited
                    grid[nx][ny] != 0) {           // Not obstacle
                    
                    visited[nx][ny] = true;
                    q.push({nx, ny});
                }
            }
        }
        steps++;  // Increment after processing each level
    }
    
    return -1; // Target not reachable
}
```

**Example Grid:**
```
S = start, T = target, 0 = obstacle, 1 = open

1 1 0 1
1 0 1 1  
1 1 1 T
S 0 1 1

BFS finds shortest path avoiding obstacles
```

---

### 6. BFS on 2D Grid (8-directional)

**Purpose:** BFS with diagonal movement allowed

**Use Cases:**
- Chess king movement
- Image processing (8-connected pixels)
- Games allowing diagonal movement

**8-Directional Movement Pattern:**
```
1 2 3
4 X 5    From X, can move to any of the 8 numbered positions
6 7 8
```

```cpp
int bfsGrid8Dir(vector<vector<int>>& grid, pair<int,int> start, pair<int,int> target) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<bool>> visited(m, vector<bool>(n, false));
    queue<pair<int,int>> q;
    
    // 8 directions: all adjacent cells (including diagonals)
    vector<pair<int,int>> directions = {
        {-1,-1}, {-1,0}, {-1,1},  // Top row
        {0,-1},          {0,1},   // Middle row (skip center)
        {1,-1},  {1,0},  {1,1}    // Bottom row
    };
    
    q.push(start);
    visited[start.first][start.second] = true;
    int steps = 0;
    
    while (!q.empty()) {
        int size = q.size();
        
        for (int i = 0; i < size; i++) {
            auto [x, y] = q.front();
            q.pop();
            
            if (make_pair(x, y) == target) {
                return steps;
            }
            
            // Check all 8 directions
            for (auto [dx, dy] : directions) {
                int nx = x + dx, ny = y + dy;
                
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && 
                    !visited[nx][ny] && grid[nx][ny] != 0) {
                    visited[nx][ny] = true;
                    q.push({nx, ny});
                }
            }
        }
        steps++;
    }
    
    return -1;
}
```

---

### 7. Multi-Source BFS

**Purpose:** BFS starting from multiple sources simultaneously

**Use Cases:**
- **Rotting Oranges:** infection spreads from multiple rotten oranges
- **Fire Spread:** fire starts from multiple points  
- **Distance to nearest facility:** hospitals, police stations
- **01 Matrix:** distance to nearest 0

**Key Insight:**
- Instead of starting from one source, start from ALL sources at once
- Add ALL sources to queue initially (they're all at distance 0)
- BFS naturally spreads outward simultaneously

```cpp
int multiSourceBFS(vector<vector<int>>& grid, vector<pair<int,int>>& sources) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<bool>> visited(m, vector<bool>(n, false));
    queue<pair<int,int>> q;
    vector<pair<int,int>> directions = {{-1,0}, {1,0}, {0,-1}, {0,1}};
    
    // CRITICAL: Add ALL sources to queue initially
    for (auto [x, y] : sources) {
        q.push({x, y});
        visited[x][y] = true;
        // All sources are at distance 0 from themselves
    }
    
    int steps = 0;
    
    while (!q.empty()) {
        int size = q.size();
        
        // Process all cells at current distance
        for (int i = 0; i < size; i++) {
            auto [x, y] = q.front();
            q.pop();
            
            // Process current cell (update values, count, etc.)
            
            for (auto [dx, dy] : directions) {
                int nx = x + dx, ny = y + dy;
                
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && 
                    !visited[nx][ny] && grid[nx][ny] != -1) {  // -1 = obstacle
                    visited[nx][ny] = true;
                    q.push({nx, ny});
                }
            }
        }
        steps++;
    }
    
    return steps - 1; // Total time taken
}
```

**Example - Rotting Oranges:**
```
2 = rotten (source), 1 = fresh, 0 = empty

Initial: 2 1 1    Sources: [(0,0)]
         1 1 0    
         0 1 1    

Time 0: [2] 1  1   (rotten oranges are sources)
        1   1  0    
        0   1  1    

Time 1: [2][2] 1   (spread to adjacent fresh oranges)  
       [2] 1  0    
        0   1  1    

Time 2: [2][2][2]  (continue spreading)
       [2][2] 0    
        0  [2] 1   

Time 3: [2][2][2]  (all fresh oranges are now rotten)
       [2][2] 0    
        0  [2][2]   

Answer: 3 minutes to rot all oranges
```

---

### 8. BFS with State Tracking

**Purpose:** BFS where same node can be visited with different states

**Use Cases:**
- **Shortest path with keys** (different keys = different states)
- **Graph coloring problems** (node + color state)
- **Time-dependent graphs** (node + time state)
- **Resource management** (node + resource state)

**When Regular BFS Isn't Enough:**
- Regular BFS: `visited[node] = true/false`
- State BFS: `visited[node][state]` or `visited["node,state"] = true/false`

```cpp
struct State {
    int node;
    int state; // Could be: key count, color, time, bitmask, etc.
    int steps;
};

int bfsWithState(vector<vector<int>>& adj, int start, int target, int targetState) {
    // Use string for visited to handle (node, state) pairs
    unordered_set<string> visited;
    queue<State> q;
    
    q.push({start, 0, 0}); // {node, initial_state, steps}
    visited.insert(to_string(start) + "," + to_string(0));
    
    while (!q.empty()) {
        State current = q.front();
        q.pop();
        
        // Check if we reached target with correct state
        if (current.node == target && current.state == targetState) {
            return current.steps;
        }
        
        for (int neighbor : adj[current.node]) {
            // STATE TRANSITION LOGIC (customize based on problem):
            int newState = current.state; 
            
            // Example state transitions:
            // newState++; // Collect item at this node
            // newState |= (1 << neighbor); // Set bit for visited neighbor  
            // newState = (newState + 1) % 3; // Cycle through colors 0,1,2
            // if (hasKey[neighbor]) newState |= keyMask[neighbor]; // Pick up key
            
            string key = to_string(neighbor) + "," + to_string(newState);
            
            if (visited.find(key) == visited.end()) {
                visited.insert(key);
                q.push({neighbor, newState, current.steps + 1});
            }
        }
    }
    
    return -1; // Target not reachable with required state
}
```

**Example - Collecting Keys:**
```
Graph: 0 -- 1 -- 2 -- 3
       Key at node 1, Door at node 3 requires key

State = number of keys collected

BFS explores:
(0, 0, 0): At node 0, 0 keys, 0 steps
(1, 1, 1): At node 1, 1 key, 1 step  (picked up key!)
(2, 1, 2): At node 2, 1 key, 2 steps
(3, 1, 3): At node 3, 1 key, 3 steps (can open door!)

Same node can be visited with different key counts:
(1, 0) vs (1, 1) are different states
```

---

## Common Mistakes & Debugging

### ❌ Common Mistakes

#### 1. **Visiting Nodes at Wrong Time**
```cpp
// ❌ Wrong - can lead to duplicate entries
int node = q.front(); q.pop();
if (!visited[node]) { visited[node] = true; ... }

// ✅ Correct - mark when adding to queue
if (!visited[neighbor]) {
    visited[neighbor] = true;
    q.push(neighbor);
}
```

#### 2. **Forgetting Boundary Checks in Grids**
```cpp
// ❌ Wrong - can access out of bounds!
if (grid[nx][ny] != 0) { ... }

// ✅ Correct - check bounds first
if (nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] != 0) { ... }
```

#### 3. **Wrong Level Counting**
```cpp
// ❌ Wrong - incrementing inside inner loop
for (int i = 0; i < size; i++) {
    // process node
    steps++; // Wrong!
}

// ✅ Correct - increment after processing each level
for (int i = 0; i < size; i++) {
    // process all nodes at current level
}
steps++; // Correct!
```

#### 4. **Infinite Loops**
- Forgetting to mark nodes as visited
- Marking visited at wrong time
- Not checking visited before adding to queue

#### 5. **Incorrect Direction Vectors**
```cpp
// ❌ Missing direction or wrong order
{{-1,0}, {1,0}, {0,1}}  // Missing one direction

// ✅ All 4 directions
{{-1,0}, {1,0}, {0,-1}, {0,1}}
```

### 🔧 Debugging Tips

#### 1. **Print Queue State**
```cpp
while (!q.empty()) {
    cout << "Queue size: " << q.size() << ", Level: " << steps << endl;
    // ... rest of BFS
}
```

#### 2. **Visualize Visited Nodes**
```cpp
// For grid problems
for (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {
        cout << (visited[i][j] ? "X" : "O") << " ";
    }
    cout << endl;
}
```

#### 3. **Check Connectivity**
- Ensure target is reachable from start
- Run basic BFS traversal first

#### 4. **Validate Input**
- Check if start/target are within bounds
- Verify graph structure

---

## Complexity Analysis

### Time Complexity
- **Adjacency List:** O(V + E)
  - V vertices, E edges
  - Each vertex visited once: O(V)
  - Each edge explored once: O(E)

- **Adjacency Matrix:** O(V²)
  - For each vertex, check all V possible neighbors

- **Grid BFS:** O(m × n)
  - m rows, n columns  
  - Each cell visited at most once
  - 4 or 8 directions per cell = constant

### Space Complexity
- **Visited array:** O(V) or O(m × n) for grids
- **Queue:** O(V) in worst case (all nodes in queue)
- **Parent array:** O(V) if used
- **Total:** O(V) for graphs, O(m × n) for grids

### State-Space BFS
- **Time:** O(V × S) where S = number of possible states
- **Space:** O(V × S) for visited tracking
- Can be exponential if state space is large!

---

## Practice Problems

### 🟢 Beginner
1. **Binary Tree Level Order Traversal** (Template 3)
2. **Number of Islands** (Template 5 + connected components)
3. **Rotting Oranges** (Template 7)

### 🟡 Intermediate
4. **Word Ladder** (Template 2 + string manipulation)
5. **01 Matrix** (Template 7 - distance to nearest 0)
6. **Knight Shortest Path** (Template 5 with custom directions)

### 🔴 Advanced
7. **Shortest Path with Keys** (Template 8)
8. **Jump Game IV** (Template 2 + state optimization)
9. **Minimum Cost to Reach Destination** (Template 8 + cost tracking)

---

## Final Tips

1. **🎯 Choose Right Template:** Match problem type to template
2. **🔍 Validate Input:** Check bounds, connectivity, edge cases
3. **🧪 Test with Small Examples:** Trace through manually first
4. **🚫 Avoid Common Mistakes:** Mark visited when enqueuing, check bounds
5. **📊 Optimize When Needed:** State representation, memory usage
6. **🔧 Debug Systematically:** Print queue states, visualize visited nodes

> **Remember:** BFS guarantees shortest path in unweighted graphs because it explores nodes level by level. The first time you reach a node, you've found the shortest path to it!

Happy coding! 🚀

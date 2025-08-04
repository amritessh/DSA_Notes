#include <iostream>
#include <vector>
#include <queue>
#include <unordered_set>
#include <unordered_map>
using namespace std;

/*
===============================================================================
                            BFS GRAPH TEMPLATES WITH EXPLANATIONS
===============================================================================

KEY CONCEPTS:
1. BFS explores nodes level by level (breadth-first)
2. Uses a QUEUE data structure (FIFO - First In, First Out)
3. Guarantees SHORTEST PATH in unweighted graphs
4. Time: O(V + E), Space: O(V) where V = vertices, E = edges

WHEN TO USE BFS:
✅ Shortest path in unweighted graphs
✅ Level-order traversal
✅ Finding minimum steps/moves
✅ Connected components  
✅ Spreading/propagation problems (like virus spread)

WHEN NOT TO USE BFS:
❌ Weighted graphs (use Dijkstra's instead)
❌ Finding ALL paths (use DFS)
❌ Deep recursion problems (use DFS)
*/

// ============================================================================
// 1. BASIC BFS TRAVERSAL - Adjacency List
// ============================================================================
/*
PURPOSE: Visit all reachable nodes from a starting point
USE CASES: 
- Print all connected nodes
- Check if graph is connected
- Count connected components

KEY POINTS:
- Mark nodes as visited BEFORE adding to queue (prevents duplicates)
- Process node when you dequeue it (not when you enqueue)
- Works for both directed and undirected graphs
*/
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
        
        cout << node << " ";   // Process current node (print, store, etc.)
        
        // Explore all neighbors
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {           // Only visit unvisited nodes
                visited[neighbor] = true;       // Mark as visited
                q.push(neighbor);               // Add to queue for processing
            }
        }
    }
}

// ============================================================================
// 2. SHORTEST PATH BFS (Unweighted Graph)
// ============================================================================
/*
PURPOSE: Find shortest distance between two nodes in unweighted graph
USE CASES:
- Minimum steps to reach target
- Word Ladder problem
- Minimum moves in games (like Chess Knight)

WHY BFS WORKS FOR SHORTEST PATH:
- BFS explores nodes level by level
- First time we reach target = shortest path
- All edges have equal weight (1)

LEVEL-BY-LEVEL PROCESSING:
- Process all nodes at distance d before any node at distance d+1
- This guarantees shortest path property
*/
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
    
    return -1; // Target not reachable from start
}

// ============================================================================
// 3. LEVEL-BY-LEVEL BFS (Track Levels)
// ============================================================================
/*
PURPOSE: Group nodes by their distance from start node
USE CASES:
- Binary tree level order traversal
- Social network: friends at distance 1, 2, 3...
- Web crawling: pages at different depths
- N-ary tree problems

DIFFERENCE FROM BASIC BFS:
- We store nodes in separate vectors for each level
- Useful when you need to process levels differently
- Common in tree problems on LeetCode

PATTERN RECOGNITION:
- If problem asks for "level", "depth", "generation" → use this template
*/
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
                    q.push(neighbor);  // These will be processed in next iteration
                }
            }
        }
        
        levels.push_back(currentLevel);  // Store completed level
    }
    
    return levels;
    
    /*
    EXAMPLE OUTPUT for tree:    1
                              / | \
                             2  3  4
                            /      |
                           5       6
    
    Result: [[1], [2,3,4], [5,6]]
    Level 0: [1]
    Level 1: [2,3,4] 
    Level 2: [5,6]
    */
}

// ============================================================================
// 4. BFS WITH PARENT TRACKING (Path Reconstruction)
// ============================================================================
/*
PURPOSE: Find shortest path AND reconstruct the actual path
USE CASES:
- Navigation systems (show route)
- Word ladder (show transformation sequence)
- Puzzle solving (show solution steps)

KEY CONCEPT - PARENT TRACKING:
- For each node, remember which node we came from
- parent[child] = current_node
- Trace back from target to start using parent array

PATH RECONSTRUCTION:
1. Start from target node
2. Follow parent pointers back to start
3. Reverse the path (since we built it backwards)

MEMORY: O(V) extra space for parent array
*/
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
    
    /*
    EXAMPLE: Finding path from 0 to 4 in graph 0-1-2-4
    
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
    */
}

// ============================================================================
// 5. BFS ON 2D GRID (4-directional)
// ============================================================================
/*
PURPOSE: BFS on 2D matrix/grid problems
USE CASES:
- Shortest path in maze
- Island problems  
- Flood fill algorithms
- Robot movement problems

4-DIRECTIONAL MOVEMENT:
- Up: (-1, 0)
- Down: (1, 0) 
- Left: (0, -1)
- Right: (0, 1)

GRID REPRESENTATION:
- grid[i][j] = 0 typically means obstacle/wall
- grid[i][j] = 1 typically means walkable/open
- Adjust based on problem requirements

BOUNDARY CHECKING:
- Always check: 0 <= x < m && 0 <= y < n
- Check validity BEFORE accessing grid[x][y]

COMMON MISTAKES:
❌ Forgetting boundary checks → Runtime error
❌ Not marking visited → Infinite loop
❌ Wrong direction vectors → Incorrect movement
*/
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
            auto [x, y] = q.front();  // C++17 structured binding
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
                    grid[nx][ny] != 0) {           // Not obstacle (adjust as needed)
                    
                    visited[nx][ny] = true;
                    q.push({nx, ny});
                }
            }
        }
        steps++;  // Increment after processing each level
    }
    
    return -1; // Target not reachable
    
    /*
    EXAMPLE GRID (0=obstacle, 1=open):
    S = start, T = target
    
    1 1 0 1
    1 0 1 1  
    1 1 1 T
    S 0 1 1
    
    BFS will find shortest path avoiding obstacles
    */
}

// ============================================================================
// 6. BFS ON 2D GRID (8-directional)
// ============================================================================
/*
PURPOSE: BFS with diagonal movement allowed
USE CASES:
- Chess king movement
- Image processing (8-connected pixels)
- Games allowing diagonal movement

8-DIRECTIONAL MOVEMENT:
- All adjacent cells including diagonals
- Horizontal/Vertical: 4 directions
- Diagonal: 4 additional directions

WHEN TO USE 8-DIR vs 4-DIR:
- 8-DIR: Chess pieces, image processing, free movement
- 4-DIR: Grid-based games, mazes, robot movement

DIAGONAL DISTANCE:
- In 8-directional, diagonal moves also count as 1 step
- If diagonal should cost more, use weighted graph algorithms
*/
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
    
    /*
    MOVEMENT PATTERN (X = current position):
    
    1 2 3
    4 X 5
    6 7 8
    
    From X, can move to any of the 8 numbered positions in one step
    */
}

// ============================================================================
// 7. MULTI-SOURCE BFS
// ============================================================================
/*
PURPOSE: BFS starting from multiple sources simultaneously
USE CASES:
- Rotting Oranges: infection spreads from multiple rotten oranges
- Fire Spread: fire starts from multiple points
- Shortest distance to any hospital/police station
- 01 Matrix: distance to nearest 0

KEY INSIGHT:
- Instead of starting from one source, start from ALL sources at once
- Add ALL sources to queue initially (they're all at distance 0)
- BFS naturally spreads outward from all sources simultaneously
- First time any cell is reached = shortest distance to ANY source

WHY IT WORKS:
- Level 0: All source cells
- Level 1: All cells adjacent to any source  
- Level 2: All cells 2 steps away from nearest source
- And so on...

ALTERNATIVE APPROACH (slower):
- Run BFS from each source separately: O(k × V × E)
- Multi-source BFS: O(V + E) where k = number of sources
*/
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
            // For "time to spread" problems, this cell gets infected at time=steps
            
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
    
    return steps - 1; // Total time taken (subtract 1 because we increment after last level)
    
    /*
    EXAMPLE - ROTTING ORANGES:
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
    */
}

// ============================================================================
// 8. BFS WITH STATE TRACKING (node + additional state)
// ============================================================================
/*
PURPOSE: BFS where same node can be visited with different states
USE CASES:
- Shortest path with keys (different keys = different states)
- Graph coloring problems (node + color state)
- Time-dependent graphs (node + time state)
- Inventory/resource management (node + resource state)

WHEN REGULAR BFS ISN'T ENOUGH:
- Regular BFS: visited[node] = true/false
- State BFS: visited[node][state] or visited["node,state"] = true/false

STATE REPRESENTATION:
- Can be single value: color, key_count, time
- Can be complex: bitmask for multiple keys, resource levels
- Use string "node,state" for complex states or hash maps

KEY INSIGHT:
- Same node with different states = different positions in search space
- Node A with key 1 ≠ Node A with key 2
- Each (node, state) combination is treated as unique

MEMORY CONSIDERATION:
- State space can be large: O(V × S) where S = number of possible states
- Use appropriate data structures for state representation
*/
struct State {
    int node;
    int state; // Could be: key count, color, time, bitmask, etc.
    int steps;
    
    // For more complex states, add more fields:
    // vector<int> resources;
    // set<int> keys;
    // int time;
};

int bfsWithState(vector<vector<int>>& adj, int start, int target, int targetState) {
    // Use string for visited to handle (node, state) pairs
    // Alternative: use map<pair<int,int>, bool> or 2D array if state space is small
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
    
    /*
    EXAMPLE - COLLECTING KEYS:
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
    */
}

// ============================================================================
// USAGE EXAMPLES & TEMPLATE SELECTION GUIDE
// ============================================================================
/*
TEMPLATE SELECTION QUICK REFERENCE:

🔍 BASIC TRAVERSAL → Template 1
   - "Visit all reachable nodes"
   - "Check connectivity" 
   - "Print/count all accessible nodes"

🎯 SHORTEST PATH → Template 2  
   - "Minimum steps/moves"
   - "Shortest distance in unweighted graph"
   - Word Ladder, Knight moves, etc.

📊 LEVEL PROCESSING → Template 3
   - "Process nodes by levels/depths"
   - "Binary tree level order"
   - "Nodes at distance k"

🛣️ PATH RECONSTRUCTION → Template 4
   - "Show the actual path/route"
   - "Print sequence of moves"
   - Navigation, puzzle solutions

🗺️ GRID PROBLEMS → Template 5/6
   - Matrix/2D array problems
   - Maze solving, island counting
   - Robot movement, flood fill

🔥 SPREADING PROBLEMS → Template 7
   - Multiple starting points
   - "Rotting oranges", fire spread
   - Distance to nearest facility

🔑 COMPLEX STATE → Template 8
   - Same node, different conditions
   - Key/door problems
   - Resource management
*/

int main() {
    cout << "=== BFS TEMPLATE DEMONSTRATION ===" << endl;
    
    // Example adjacency list: 0-1-2-3
    //                        |   |
    //                        4---5
    vector<vector<int>> adj = {
        {1, 4},    // 0 connects to 1, 4
        {0, 2},    // 1 connects to 0, 2  
        {1, 3, 5}, // 2 connects to 1, 3, 5
        {2},       // 3 connects to 2
        {0, 5},    // 4 connects to 0, 5
        {2, 4}     // 5 connects to 2, 4
    };
    
    cout << "\nGraph structure:" << endl;
    cout << "0 -- 1 -- 2 -- 3" << endl;
    cout << "|    |    |" << endl; 
    cout << "4 ------- 5" << endl;
    
    // Template 1: Basic Traversal
    cout << "\n1. BFS Traversal from node 0: ";
    bfsTraversal(adj, 0);
    cout << endl;
    
    // Template 2: Shortest Path
    cout << "2. Shortest path from 0 to 3: " << shortestPath(adj, 0, 3) << " steps" << endl;
    
    // Template 3: Level-by-level
    vector<vector<int>> levels = bfsLevels(adj, 0);
    cout << "3. Nodes by levels from 0:" << endl;
    for (int i = 0; i < levels.size(); i++) {
        cout << "   Level " << i << ": ";
        for (int node : levels[i]) {
            cout << node << " ";
        }
        cout << endl;
    }
    
    // Template 4: Path Reconstruction  
    vector<int> path = bfsPath(adj, 0, 3);
    cout << "4. Actual path from 0 to 3: ";
    for (int i = 0; i < path.size(); i++) {
        cout << path[i];
        if (i < path.size() - 1) cout << " -> ";
    }
    cout << endl;
    
    cout << "\n=== GRID BFS EXAMPLE ===" << endl;
    // Template 5: Grid BFS
    vector<vector<int>> maze = {
        {1, 1, 0, 1},
        {1, 0, 1, 1},
        {1, 1, 1, 0},
        {0, 1, 1, 1}
    };
    
    cout << "Maze (1=open, 0=wall):" << endl;
    for (auto& row : maze) {
        for (int cell : row) {
            cout << cell << " ";
        } 
        cout << endl;
    }
    
    int gridSteps = bfsGrid(maze, {0, 0}, {3, 3});
    cout << "Shortest path from (0,0) to (3,3): " << gridSteps << " steps" << endl;
    
    return 0;
}

/*
===============================================================================
                            COMMON MISTAKES & DEBUGGING TIPS
===============================================================================

❌ COMMON MISTAKES:

1. VISITING NODES AT WRONG TIME:
   Wrong: Mark visited when dequeuing
   ✅ Correct: Mark visited when enqueuing
   
   // Wrong - can lead to duplicate entries
   int node = q.front(); q.pop();
   if (!visited[node]) { visited[node] = true; ... }
   
   // Correct - mark when adding to queue
   if (!visited[neighbor]) {
       visited[neighbor] = true;
       q.push(neighbor);
   }

2. FORGETTING BOUNDARY CHECKS IN GRIDS:
   ❌ if (grid[nx][ny] != 0) { ... }  // Can access out of bounds!
   ✅ if (nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] != 0) { ... }

3. WRONG LEVEL COUNTING:
   ❌ Incrementing steps inside the inner loop
   ✅ Increment steps after processing each level
   
4. INFINITE LOOPS:
   - Forgetting to mark nodes as visited
   - Marking visited at wrong time
   - Not checking visited before adding to queue

5. INCORRECT DIRECTION VECTORS:
   ❌ {{-1,0}, {1,0}, {0,1}, {0,-1}}  // Missing direction
   ✅ {{-1,0}, {1,0}, {0,-1}, {0,1}}  // All 4 directions

🔧 DEBUGGING TIPS:

1. PRINT QUEUE STATE:
   while (!q.empty()) {
       cout << "Queue size: " << q.size() << ", Level: " << steps << endl;
       // ... rest of BFS
   }

2. VISUALIZE VISITED NODES:
   // For grid problems
   for (int i = 0; i < m; i++) {
       for (int j = 0; j < n; j++) {
           cout << (visited[i][j] ? "X" : "O") << " ";
       }
       cout << endl;
   }

3. CHECK CONNECTIVITY:
   // Ensure target is reachable from start
   // Run basic BFS traversal first

4. VALIDATE INPUT:
   // Check if start/target are within bounds
   // Verify graph structure

===============================================================================
                                COMPLEXITY ANALYSIS
===============================================================================

TIME COMPLEXITY:
- Adjacency List: O(V + E)
  - V vertices, E edges
  - Each vertex visited once: O(V)
  - Each edge explored once: O(E)
  
- Adjacency Matrix: O(V²)
  - For each vertex, check all V possible neighbors
  
- Grid BFS: O(m × n)
  - m rows, n columns
  - Each cell visited at most once
  - 4 or 8 directions per cell = constant

SPACE COMPLEXITY:
- Visited array: O(V) or O(m × n) for grids
- Queue: O(V) in worst case (all nodes in queue)
- Parent array (if used): O(V)
- Total: O(V) for graphs, O(m × n) for grids

STATE-SPACE BFS:
- Time: O(V × S) where S = number of possible states
- Space: O(V × S) for visited tracking
- Can be exponential if state space is large!

===============================================================================
                            PROBLEM PATTERN RECOGNITION
===============================================================================

🔍 KEYWORDS THAT SUGGEST BFS:

"Shortest/Minimum":
- Shortest path, minimum steps, minimum time
- Closest/nearest (in unweighted graph)

"Level/Distance":
- Nodes at distance k
- Level order traversal
- Generation, depth

"Spread/Propagation":
- Fire spread, infection spread
- Multiple sources
- Time-based expansion

"Grid/Matrix":
- 2D array problems
- Robot movement
- Maze solving

"State Space":
- Different conditions for same node
- Keys and doors
- Resource management

🎯 BFS vs DFS vs DIJKSTRA:

BFS: ✅ Unweighted shortest path ✅ Level processing ✅ Spreading problems
DFS: ✅ Find any path ✅ Backtracking ✅ Connected components  
Dijkstra: ✅ Weighted shortest path ✅ Non-negative weights

===============================================================================
                                PRACTICE PROBLEMS
===============================================================================

BEGINNER:
1. Binary Tree Level Order Traversal (Template 3)
2. Number of Islands (Template 5 + connected components)
3. Rotting Oranges (Template 7)

INTERMEDIATE:  
4. Word Ladder (Template 2 + string manipulation)
5. 01 Matrix (Template 7 - distance to nearest 0)
6. Knight Shortest Path (Template 5 with custom directions)

ADVANCED:
7. Shortest Path with Keys (Template 8)
8. Jump Game IV (Template 2 + state optimization)
9. Minimum Cost to Reach Destination (Template 8 + cost tracking)

===============================================================================
                                FINAL TIPS
===============================================================================

1. 🎯 CHOOSE RIGHT TEMPLATE: Match problem type to template
2. 🔍 VALIDATE INPUT: Check bounds, connectivity, edge cases
3. 🧪 TEST WITH SMALL EXAMPLES: Trace through manually first
4. 🚫 AVOID COMMON MISTAKES: Mark visited when enqueuing, check bounds
5. 📊 OPTIMIZE WHEN NEEDED: State representation, memory usage
6. 🔧 DEBUG SYSTEMATICALLY: Print queue states, visualize visited nodes

Remember: BFS guarantees shortest path in unweighted graphs because it 
explores nodes level by level. The first time you reach a node, you've 
found the shortest path to it!

Happy coding! 🚀
*/

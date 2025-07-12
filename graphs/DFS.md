In Graph theory, the depth-first search algorithm (abbreviated as DFS) is mainly used to:

Traverse all vertices in a “graph”;
Traverse all paths between any two vertices in a “graph”.

When we traverse an adjacent vertex, we completely finish the traversal of all vertices reachable through that adjacent vertex.

DFS - Stack


// 🎯 COMPLETE DFS TEMPLATES: RECURSIVE + ITERATIVE
// Memorize both patterns and choose based on problem constraints!

#include <vector>
#include <stack>
#include <queue>
using namespace std;

// =====================================
// 🌟 UNIVERSAL RECURSIVE DFS TEMPLATE
// =====================================
class RecursiveDFS {
private:
    // 📋 Common variables
    vector<vector<int>> directions = {{-1,0}, {1,0}, {0,-1}, {0,1}};
    int rows, cols;
    
    // 🔥 MASTER RECURSIVE TEMPLATE (MEMORIZE THIS!)
    bool dfs(GRAPH_TYPE& graph, int node, VISITED_TYPE& visited, PARAMS...) {
        // 1️⃣ BASE CASES
        if (OUT_OF_BOUNDS_CHECK) return false;
        if (visited[node] || INVALID_CONDITION) return false;
        if (TARGET_REACHED) return true;
        
        // 2️⃣ MARK VISITED
        visited[node] = true;
        
        // 3️⃣ PROCESS CURRENT NODE
        PROCESS_CURRENT_NODE;
        
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
};

// =====================================
// 🌟 UNIVERSAL ITERATIVE DFS TEMPLATE
// =====================================
class IterativeDFS {
private:
    // 🔥 MASTER ITERATIVE TEMPLATE (MEMORIZE THIS!)
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
            PROCESS_CURRENT_NODE;
            
            // 5️⃣ ADD NEIGHBORS TO STACK
            for (int neighbor : getNeighbors(node)) {
                if (!visited[neighbor]) {
                    stk.push(neighbor);
                }
            }
        }
        
        return false;
    }
};

// =====================================
// 🎨 CONCRETE EXAMPLES: GRID PROBLEMS
// =====================================

// 1️⃣ NUMBER OF ISLANDS - RECURSIVE
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

// 1️⃣ NUMBER OF ISLANDS - ITERATIVE
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

// =====================================
// 🎨 CONCRETE EXAMPLES: ADJACENCY LIST
// =====================================

// 2️⃣ PATH EXISTS - RECURSIVE
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

// 2️⃣ PATH EXISTS - ITERATIVE
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

// =====================================
// 🎨 CONCRETE EXAMPLES: CYCLE DETECTION
// =====================================

// 3️⃣ CYCLE DETECTION - RECURSIVE
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

// 3️⃣ CYCLE DETECTION - ITERATIVE
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

// =====================================
// 🎨 CONCRETE EXAMPLES: PATH FINDING
// =====================================

// 4️⃣ FIND PATH - RECURSIVE
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

// 4️⃣ FIND PATH - ITERATIVE (More Complex)
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

// =====================================
// 🎯 WHEN TO USE WHICH?
// =====================================

/*
🚀 RECURSIVE DFS:
✅ Pros:
- Cleaner, more intuitive code
- Natural backtracking
- Easy to understand
- Perfect for tree problems

❌ Cons:
- Stack overflow for deep graphs
- Less control over memory
- Hidden overhead

🚀 ITERATIVE DFS:
✅ Pros:
- No stack overflow
- Better memory control
- Can handle very deep graphs
- More efficient for large inputs

❌ Cons:
- More complex code
- Harder to implement backtracking
- Less intuitive

🎯 DECISION GUIDE:
- Use RECURSIVE for: Trees, small graphs, problems requiring backtracking
- Use ITERATIVE for: Large graphs, deep structures, memory constraints
*/

// =====================================
// 🧠 MEMORIZATION CHEAT SHEET
// =====================================

/*
🔥 RECURSIVE PATTERN:
1. Base cases (bounds, visited, target)
2. Mark visited
3. Process node
4. Recurse on neighbors
5. Backtrack (if needed)

🔥 ITERATIVE PATTERN:
1. Initialize stack with starting node
2. While stack not empty:
   - Pop node
   - Skip if visited
   - Process node
   - Mark visited
   - Push neighbors

🎯 KEY DIFFERENCES:
- Recursive: implicit call stack
- Iterative: explicit stack data structure
- Recursive: natural backtracking
- Iterative: manual backtracking (store state in stack)
*/



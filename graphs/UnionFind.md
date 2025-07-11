Given the vertices and edges between them, how could we quickly check whether two vertices are connected?

The primary use of disjoint sets is to address the connectivity between the components of a network. The “network“ here can be a computer network or a social network. For instance, we can use a disjoint set to determine if two people share a common ancestor.

Parent node: the direct parent node of a vertex.
Root node: a node without a parent node; it can be viewed as the parent node of itself.

The find function finds the root node of a given vertex.
The union function unions two vertices and makes their root nodes the same.

the “rank” refers to the height of each vertex. When we union two vertices, instead of always picking the root of x (or y, it doesn't matter as long as we're consistent) as the new root node, we choose the root node of the vertex with a larger “rank”. We will merge the shorter tree under the taller tree and assign the root node of the taller tree as the root node for both vertices. In this way, we effectively avoid the possibility of connecting all vertices into a straight line. This optimization is called the “disjoint set” with union by rank.

The main idea of a “disjoint set” is to have all connected vertices have the same parent node or root node, whether directly or indirectly connected. To check if two vertices are connected, we only need to check if they have the same root node.

The two most important functions for the “disjoint set” data structure are the find function and the union function. The find function locates the root node of a given vertex. The union function connects two previously unconnected vertices by giving them the same root node. There is another important function named connected, which checks the “connectivity” of two vertices. The find and union functions are essential for any question that uses the “disjoint set” data structure.

memorize the implementation of “disjoint set with path compression and union by rank”.

# 🎯 Union Find Template - Clean Style

## Overview

**Purpose:** Efficiently manage disjoint sets and answer connectivity queries

**Main Operations:**
- `find(x)`: Which set does x belong to?
- `unionSet(x,y)`: Merge the sets containing x and y

**Key Optimizations:**
- **Path Compression**: Makes `find()` nearly O(1)
- **Union by Rank**: Keeps trees shallow, prevents worst-case O(n) chains

**Complexity:**
- **Time**: O(α(n)) per operation where α is inverse Ackermann (nearly constant)
- **Space**: O(n)

## Template Implementation

```cpp
class UnionFind {
private:
    vector<int> root;   // root[i] = root of the tree containing i
    vector<int> rank;   // rank[i] = height of tree rooted at i
    int count = 0;      // Number of disjoint sets/components remaining

public:
    /**
     * CONSTRUCTOR: Initialize n separate disjoint sets
     * - root[i] = i: each element is its own root initially
     * - rank[i] = 1: each single node has rank 1 (tree size)
     * - count = sz: initially we have 'sz' separate components
     * Time: O(n)
     */

    UnionFind(int size){
        root.resize(size);
        rank.resize(size,0);
        count = size;
        for(int i = 0 ; i < size; i++){
            root[i] = i;
        }
    }
    
    
    /**
     * FIND: Find the root/representative of the set containing x
     * OPTIMIZATION: Path Compression - make all nodes on path point directly to root
     * 
     * Why Path Compression?
     * - Without it: find() could be O(n) in worst case (long chain)
     * - With it: find() becomes nearly O(1) amortized
     * 
     * How it works:
     * - While going up to find root, make every node point directly to root
     * - Next time we query any node on this path, it's O(1)
     * 
     * Time: O(α(n)) amortized (nearly constant)
     */
    int find(int x){
        if(root[x]!=x) root[x]=find(root[x]);
        return root[x];
    }
    
    /**
     * UNIONSET: Merge the sets containing x and y
     * OPTIMIZATION: Union by Rank - attach smaller tree to larger tree
     * 
     * Why Union by Rank?
     * - Prevents creating long chains which would make find() slow
     * - Always attach the tree with smaller rank to the tree with larger rank
     * - Keeps overall tree height logarithmic
     * 
     * Cases:
     * 1. x and y already in same set → do nothing
     * 2. Different ranks → attach smaller rank tree to larger rank tree
     * 3. Same ranks → attach either to other, increment rank of new root
     * 
     * Time: O(α(n)) amortized
     */
    void unionSet(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        // Already in same set - no union needed
        if (rootX != rootY) {
            // Union by rank: attach smaller tree to larger tree
            if (rank[rootX] > rank[rootY]) {
                root[rootY] = rootX;        // Make rootX the parent of rootY
            } else if (rank[rootX] < rank[rootY]) {
                root[rootX] = rootY;        // Make rootY the parent of rootX
            } else {
                // Same rank: arbitrarily choose one as parent, increment its rank
                root[rootY] = rootX;
                rank[rootX] += 1;           // New root's rank increases by 1
            }
            // Two sets merged into one → decrease component count
            count--;
        }
    }
    
    /**
     * GETCOUNT: Return number of disjoint sets remaining
     * Use cases:
     * - Count connected components in a graph
     * - Count number of islands
     * - Count number of friend groups
     * 
     * Time: O(1)
     */
    int getCount() {
        return count;
    }
    
    /**
     * ISCONNECTED: Check if x and y are in the same set
     * Use cases:
     * - Cycle detection: if we're about to connect two nodes that are already connected
     * - Query if two nodes are reachable from each other
     * 
     * Time: O(α(n)) amortized
     */
    bool isConnected(int x, int y) {
        return find(x) == find(y);
    }
};
```

## Common Usage Patterns

### Pattern 1: Count Connected Components

```cpp
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        if (isConnected.size() == 0) {
            return 0;
        }
        int n = isConnected.size();
        UnionFind uf(n);
        
        // Optimize: only check upper triangle since matrix is symmetric
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    uf.unionSet(i, j);
                }
            }
        }
        return uf.getCount();
    }
};
```

### Pattern 2: Cycle Detection

```cpp
class Solution {
public:
    bool validTree(int n, vector<vector<int>>& edges) {
        if (edges.size() != n - 1) return false;
        
        UnionFind uf(n);
        for (auto& edge : edges) {
            if (uf.isConnected(edge[0], edge[1])) {
                return false;  // Cycle detected
            }
            uf.unionSet(edge[0], edge[1]);
        }
        return uf.getCount() == 1;
    }
};
```

### Pattern 3: Redundant Connection

```cpp
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        UnionFind uf(edges.size());
        
        for (auto& edge : edges) {
            if (uf.isConnected(edge[0] - 1, edge[1] - 1)) {
                return edge;  // This edge creates a cycle
            }
            uf.unionSet(edge[0] - 1, edge[1] - 1);
        }
        return {};
    }
};
```

### Pattern 4: Minimum Spanning Tree (Kruskal's)

```cpp
class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        int n = points.size();
        
        // Generate all possible edges with their weights
        vector<array<int, 3>> edges;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int weight = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1]);
                edges.push_back({weight, i, j});
            }
        }
        
        // Sort edges by weight (Kruskal's algorithm)
        sort(edges.begin(), edges.end());
        
        UnionFind uf(n);
        int totalCost = 0;
        
        // Add edges in order of increasing weight
        for (auto& edge : edges) {
            int weight = edge[0], u = edge[1], v = edge[2];
            
            // Only add edge if it connects different components
            if (!uf.isConnected(u, v)) {
                uf.unionSet(u, v);
                totalCost += weight;
                
                // Early termination: MST has exactly n-1 edges
                if (uf.getCount() == 1) break;
            }
        }
        
        return totalCost;
    }
};
```

### Pattern 5: Dynamic Connectivity

```cpp
class Solution {
public:
    vector<int> numIslands2(int m, int n, vector<vector<int>>& positions) {
        UnionFind uf(m * n);
        vector<vector<bool>> grid(m, vector<bool>(n, false));
        vector<int> result;
        
        int directions[4][2] = {{-1,0}, {1,0}, {0,-1}, {0,1}};
        
        for (auto& pos : positions) {
            int r = pos[0], c = pos[1];
            
            if (grid[r][c]) {
                result.push_back(uf.getCount());
                continue;
            }
            
            grid[r][c] = true;
            
            // Connect to adjacent land cells
            for (int i = 0; i < 4; i++) {
                int nr = r + directions[i][0];
                int nc = c + directions[i][1];
                
                if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc]) {
                    uf.unionSet(r * n + c, nr * n + nc);
                }
            }
            
            result.push_back(uf.getCount());
        }
        
        return result;
    }
};
```

## When to Use Union Find

### ✅ Perfect for:
- Counting connected components
- Cycle detection in undirected graphs
- Minimum Spanning Tree (Kruskal's)
- Dynamic connectivity queries
- Percolation problems
- Friend/group relationship problems

### ❌ Not suitable for:
- Shortest path problems (use BFS/Dijkstra)
- Finding actual paths (only tells if connected)
- Directed graph problems (Union Find works on undirected)
- Problems requiring tree structure

## Recognition Patterns

Look for these phrases in problems:
- "How many connected components?"
- "Are these two elements connected?"
- "Will adding this edge create a cycle?"
- "Minimum cost to connect all points"
- "Group elements by some relationship"

## Key Features

1. **Clean Initialization**: Variables initialized at declaration in private section
2. **Modern C++ Style**: Normal constructor initialization (no initializer lists)
3. **Path Compression**: Single-line optimization for find()
4. **Union by Rank**: Keeps trees balanced for optimal performance
5. **Component Counting**: Automatic tracking of connected components

## Complexity Analysis

**Time Complexity:** O(α(n)) per operation
- α(n) is inverse Ackermann function
- For all practical purposes, α(n) ≤ 4 for n ≤ 2^65536
- So effectively O(1) per operation

**Space Complexity:** O(n)
- root array: O(n)
- rank array: O(n)
- Total: O(n)

**Why so fast?**
- Path compression makes find() nearly O(1)
- Union by rank keeps trees shallow
- Together they give nearly constant time per operation

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

# 🎯 Union Find (Disjoint Set) Template

## Overview

**Purpose:** Efficiently manage disjoint sets and answer connectivity queries

**Main Operations:**
- `find(x)`: Which set does x belong to?
- `union_set(x,y)`: Merge the sets containing x and y

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
    vector<int> parent;  // parent[i] = parent of node i (if parent[i] == i, then i is root)
    vector<int> rank;    // rank[i] = approximate depth of tree rooted at i (used for union by rank)
    int count;           // Number of disjoint sets/components remaining

public:
    /**
     * CONSTRUCTOR: Initialize n separate disjoint sets
     * Initially: each element is its own parent (each element is a separate set)
     * Time: O(n)
     */
    UnionFind(int size) {
        parent.resize(size);
        rank.resize(size, 0);    // All ranks start at 0 (single nodes have rank 0)
        count = size;            // Initially we have 'size' separate components
        
        // Each element is its own parent initially (each is a separate set)
        for(int i = 0; i < size; i++) {
            parent[i] = i;
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
    int find(int x) {
        if(parent[x] != x) {
            // Path compression: make x point directly to root
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    /**
     * UNION_SET: Merge the sets containing x and y
     * OPTIMIZATION: Union by Rank - attach smaller tree to larger tree
     * 
     * Why Union by Rank?
     * - Prevents creating long chains which would make find() slow
     * - Always attach the shorter tree to the taller tree
     * - Keeps overall tree height logarithmic
     * 
     * Cases:
     * 1. x and y already in same set → do nothing
     * 2. Different ranks → attach smaller rank tree to larger rank tree
     * 3. Same ranks → attach either to other, increment rank of new root
     * 
     * Time: O(α(n)) amortized
     */
    void union_set(int x, int y) {
        int xset = find(x), yset = find(y);
        
        // Already in same set - no union needed
        if(xset == yset) return;
        
        // Union by rank: attach smaller tree to larger tree
        if(rank[xset] < rank[yset]) {
            parent[xset] = yset;        // Make yset the parent of xset
        } else if(rank[xset] > rank[yset]) {
            parent[yset] = xset;        // Make xset the parent of yset
        } else {
            // Same rank: arbitrarily choose one as parent, increment its rank
            parent[yset] = xset;
            rank[xset]++;               // New root's rank increases by 1
        }
        
        // Two sets merged into one → decrease component count
        count--;
    }
    
    /**
     * GET_COUNT: Return number of disjoint sets remaining
     * Use cases:
     * - Count connected components in a graph
     * - Count number of islands
     * - Count number of friend groups
     * 
     * Time: O(1)
     */
    int get_count() {
        return count;
    }
    
    /**
     * IS_CONNECTED: Check if x and y are in the same set
     * Use cases:
     * - Cycle detection: if we're about to connect two nodes that are already connected
     * - Query if two nodes are reachable from each other
     * 
     * Time: O(α(n)) amortized
     */
    bool is_connected(int x, int y) {
        return find(x) == find(y);
    }
};
```

## Common Usage Patterns

### Pattern 1: Count Connected Components

```cpp
UnionFind dsu(n);
for(auto& edge : edges) {
    dsu.union_set(edge[0], edge[1]);
}
return dsu.get_count();
```

### Pattern 2: Cycle Detection

```cpp
UnionFind dsu(n);
for(auto& edge : edges) {
    if(dsu.is_connected(edge[0], edge[1])) {
        return true;  // Cycle detected
    }
    dsu.union_set(edge[0], edge[1]);
}
return false;
```

### Pattern 3: Minimum Spanning Tree (Kruskal's)

```cpp
sort(edges.begin(), edges.end(), [](const auto& a, const auto& b) {
    return a[2] < b[2];
});

UnionFind dsu(n);
int total_cost = 0;
for(auto& edge : edges) {
    if(!dsu.is_connected(edge[0], edge[1])) {
        dsu.union_set(edge[0], edge[1]);
        total_cost += edge[2];
    }
}
```

### Pattern 4: Dynamic Connectivity

```cpp
UnionFind dsu(n);
for(auto& operation : operations) {
    dsu.union_set(operation[0], operation[1]);
    results.push_back(dsu.get_count());
}
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

## Complexity Analysis

**Time Complexity:** O(α(n)) per operation
- α(n) is inverse Ackermann function
- For all practical purposes, α(n) ≤ 4 for n ≤ 2^65536
- So effectively O(1) per operation

**Space Complexity:** O(n)
- parent array: O(n)
- rank array: O(n)
- Total: O(n)

**Why so fast?**
- Path compression makes find() nearly O(1)
- Union by rank keeps trees shallow
- Together they give nearly constant time per operation


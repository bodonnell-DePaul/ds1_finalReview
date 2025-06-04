# Lecture Notes on Sorting Algorithms and Union-Find Data Structure

## Overview
This document covers fundamental algorithms in computer science: three popular sorting algorithms (Bubble Sort, Heap Sort, and Quick Sort) and the Union-Find data structure. Each algorithm has its own strengths and weaknesses, making them suitable for different scenarios.

## Part I: Sorting Algorithms

### 1. Bubble Sort
#### Implementation
Bubble Sort is a simple comparison-based sorting algorithm. It repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The process is repeated until the list is sorted.

#### Time & Space Complexity
- **Time Complexity**: O(n²) average and worst case, O(n) best case with optimization
- **Space Complexity**: O(1) - in-place sorting
- **Stability**: Stable (maintains relative order of equal elements)

#### When to Use
- Best for small datasets or when the data is nearly sorted.
- Easy to implement and understand.
- Educational purposes where simplicity matters more than efficiency.

#### Practical Applications
- Educational purposes to teach sorting concepts.
- Embedded systems with severe memory constraints.
- Simple data validation where performance isn't critical.

### 2. Heap Sort
#### Implementation
Heap Sort is a comparison-based sorting algorithm that uses a binary heap data structure. It first builds a max heap from the input data, then repeatedly extracts the maximum element from the heap and rebuilds the heap until the array is sorted.

#### Time & Space Complexity
- **Time Complexity**: O(n log n) in all cases - guaranteed performance
- **Space Complexity**: O(1) - in-place sorting
- **Stability**: Not stable

#### When to Use
- Suitable for large datasets.
- When guaranteed O(n log n) performance is required.
- Memory-constrained environments.
- Real-time systems requiring predictable performance.

#### Practical Applications
- Used in applications where memory usage is a concern, as it sorts in-place.
- Operating system job scheduling algorithms.
- Priority queue implementations.
- Safety-critical systems requiring predictable timing.

### 3. Quick Sort
#### Implementation
Quick Sort is a highly efficient sorting algorithm that uses a divide-and-conquer approach. It selects a 'pivot' element from the array and partitions the other elements into two sub-arrays according to whether they are less than or greater than the pivot. The sub-arrays are then sorted recursively.

#### Time & Space Complexity
- **Time Complexity**: O(n log n) average case, O(n²) worst case
- **Space Complexity**: O(log n) due to recursion stack
- **Stability**: Not stable

#### When to Use
- Generally faster than other O(n log n) algorithms like Heap Sort and Merge Sort.
- Effective for large datasets with random distribution.
- When average-case performance is critical.

#### Practical Applications
- Widely used in various applications, including database sorting and in-memory sorting.
- Java's `Arrays.sort()` for primitive types.
- C++ standard library implementations.
- Graphics programming for z-buffer sorting.

## Part II: Union-Find Data Structure

### Overview
Union-Find, also known as Disjoint Set Union (DSU), is a data structure that efficiently tracks a set of elements partitioned into disjoint (non-overlapping) subsets. It's particularly powerful for solving connectivity problems in graphs and networks.

### Core Operations
1. **Find(x)**: Determines which subset element x belongs to (finds the root/representative)
2. **Union(x, y)**: Merges the subsets containing elements x and y
3. **Connected(x, y)**: Checks if two elements are in the same subset

### Basic Implementation in Java
```java
public class UnionFind {
    private int[] parent;
    private int[] rank;
    private int components;
    
    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        components = n;
        
        // Initially, each element is its own parent
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            rank[i] = 0;
        }
    }
    
    // Find with path compression
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    // Union by rank
    public boolean union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) return false;
        
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
    
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
    
    public int getComponents() {
        return components;
    }
}
```

### Optimizations

#### 1. Path Compression
- **Purpose**: Makes trees flatter by making every node point directly to the root
- **Implementation**: During find operations, update parent pointers
- **Effect**: Reduces time complexity to nearly O(1) amortized

#### 2. Union by Rank/Size
- **Purpose**: Keeps trees balanced by attaching smaller tree under larger tree
- **Implementation**: Track rank (tree height) or size of each component
- **Effect**: Prevents degeneration to linear chains

### Time Complexity Analysis
- **Without optimizations**: O(n) per operation in worst case
- **With path compression only**: O(log n) amortized per operation
- **With union by rank only**: O(log n) per operation
- **With both optimizations**: O(α(n)) amortized per operation
  - α(n) is the inverse Ackermann function, practically constant for all realistic inputs

### Practical Applications in Java

#### 1. Kruskal's Minimum Spanning Tree Algorithm
```java
public class KruskalMST {
    static class Edge implements Comparable<Edge> {
        int src, dest, weight;
        
        public int compareTo(Edge other) {
            return Integer.compare(this.weight, other.weight);
        }
    }
    
    public static List<Edge> findMST(int vertices, List<Edge> edges) {
        Collections.sort(edges);
        UnionFind uf = new UnionFind(vertices);
        List<Edge> mst = new ArrayList<>();
        
        for (Edge edge : edges) {
            if (uf.union(edge.src, edge.dest)) {
                mst.add(edge);
                if (mst.size() == vertices - 1) break;
            }
        }
        return mst;
    }
}
```

#### 2. Dynamic Connectivity Problems
- **Network connectivity**: Determine if two computers can communicate
- **Social networks**: Find connected friend groups
- **Real-time graph updates**: Handle edge additions efficiently

#### 3. Connected Components in Graphs
```java
public static int countConnectedComponents(int n, int[][] edges) {
    UnionFind uf = new UnionFind(n);
    
    for (int[] edge : edges) {
        uf.union(edge[0], edge[1]);
    }
    
    return uf.getComponents();
}
```

#### 4. Percolation Theory
Used in physics and material science to model systems like:
- Water flow through porous materials
- Electrical conductivity in composite materials
- Disease spread in populations

```java
public class Percolation {
    private boolean[][] grid;
    private UnionFind uf;
    private int size;
    private int virtualTop, virtualBottom;
    
    public Percolation(int n) {
        size = n;
        grid = new boolean[n][n];
        uf = new UnionFind(n * n + 2); // +2 for virtual sites
        virtualTop = n * n;
        virtualBottom = n * n + 1;
    }
    
    public void open(int row, int col) {
        if (grid[row][col]) return;
        
        grid[row][col] = true;
        int site = row * size + col;
        
        // Connect to virtual top/bottom
        if (row == 0) uf.union(site, virtualTop);
        if (row == size - 1) uf.union(site, virtualBottom);
        
        // Connect to open neighbors
        int[][] directions = {{-1,0}, {1,0}, {0,-1}, {0,1}};
        for (int[] dir : directions) {
            int newRow = row + dir[0];
            int newCol = col + dir[1];
            if (isValid(newRow, newCol) && grid[newRow][newCol]) {
                uf.union(site, newRow * size + newCol);
            }
        }
    }
    
    public boolean percolates() {
        return uf.connected(virtualTop, virtualBottom);
    }
}
```

#### 5. Account Merging Problem
Common in data processing where you need to merge user accounts based on shared email addresses:

```java
public static List<List<String>> accountsMerge(List<List<String>> accounts) {
    Map<String, Integer> emailToId = new HashMap<>();
    Map<String, String> emailToName = new HashMap<>();
    int id = 0;
    
    // Assign IDs to emails
    for (List<String> account : accounts) {
        String name = account.get(0);
        for (int i = 1; i < account.size(); i++) {
            String email = account.get(i);
            if (!emailToId.containsKey(email)) {
                emailToId.put(email, id++);
            }
            emailToName.put(email, name);
        }
    }
    
    UnionFind uf = new UnionFind(id);
    
    // Union emails in same account
    for (List<String> account : accounts) {
        for (int i = 2; i < account.size(); i++) {
            uf.union(emailToId.get(account.get(1)), 
                    emailToId.get(account.get(i)));
        }
    }
    
    // Group and format results
    Map<Integer, List<String>> groups = new HashMap<>();
    for (String email : emailToId.keySet()) {
        int root = uf.find(emailToId.get(email));
        groups.computeIfAbsent(root, k -> new ArrayList<>()).add(email);
    }
    
    List<List<String>> result = new ArrayList<>();
    for (List<String> emails : groups.values()) {
        Collections.sort(emails);
        List<String> account = new ArrayList<>();
        account.add(emailToName.get(emails.get(0)));
        account.addAll(emails);
        result.add(account);
    }
    
    return result;
}
```

### When to Use Union-Find
✅ **Ideal for:**
- Dynamic connectivity queries
- Cycle detection in undirected graphs
- Finding connected components
- Clustering problems
- Kruskal's MST algorithm
- Percolation problems

❌ **Not suitable for:**
- Finding shortest paths
- Directed graph problems
- When you need to remove edges (standard implementation doesn't support)
- Small datasets where simpler approaches suffice

### Common Interview Problems Using Union-Find
1. **Number of Islands** - Count connected components in a 2D grid
2. **Friend Circles** - Find groups of friends in social networks
3. **Redundant Connection** - Find the edge that creates a cycle
4. **Accounts Merge** - Merge user accounts with common emails
5. **Most Stones Removed** - Maximum stones that can be removed while maintaining connectivity

### Advanced Variants
1. **Weighted Union-Find**: Tracks component sizes for size-based unions
2. **Union-Find with Rollback**: Supports undoing union operations
3. **Persistent Union-Find**: Maintains history of all operations
4. **Union-Find with Path Splitting**: Alternative to path compression

## Algorithm Comparison Summary

| Algorithm | Time Complexity | Space | Stability | Best Use Case |
|-----------|----------------|-------|-----------|---------------|
| Bubble Sort | O(n²) avg/worst | O(1) | Stable | Education, tiny datasets |
| Quick Sort | O(n log n) avg | O(log n) | Unstable | General purpose, large random data |
| Heap Sort | O(n log n) all | O(1) | Unstable | Guaranteed performance |
| Union-Find | O(α(n)) amortized | O(n) | N/A | Connectivity problems |

## Conclusion
Understanding these fundamental algorithms is crucial for software development and problem-solving:

- **Sorting algorithms** each have specific strengths: Bubble Sort for education, Quick Sort for general use, and Heap Sort for guaranteed performance.
- **Union-Find** is indispensable for connectivity problems and graph algorithms, offering nearly constant-time operations with proper optimizations.
- The choice of algorithm should always consider the specific requirements: dataset size, performance guarantees, memory constraints, and stability needs.
- These algorithms form the foundation for more complex data structures and algorithms in computer science.
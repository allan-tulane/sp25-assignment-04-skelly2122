# CMPS 2200 Assignment 5
## Answers

**Name:** Samuel Kelly


- **1a.** The maximum depth of a d-ary heap with n elements is ⌈log_d(n)⌉. 

  This is because in a d-ary heap, each node can have up to d children. At depth 0 (the root), we have 1 node. At depth 1, we can have up to d nodes. At depth 2, we can have up to d² nodes, and so on. At depth h, we can have up to d^h nodes.

  The total number of nodes in a perfect d-ary tree of height h is:
  1 + d + d² + ... + d^h = (d^(h+1) - 1)/(d - 1)

  For n elements, we need:
  n ≤ (d^(h+1) - 1)/(d - 1)

  Solving for h, we get h ≥ log_d((n(d-1) + 1)) - 1
  
  Which simplifies to h = ⌈log_d(n)⌉ for the maximum depth.


- **1b.** For a d-ary heap:

  **delete-min operation**: O(d log_d n)
  - Remove the root: O(1)
  - Move the rightmost leaf to the root: O(1)
  - Sift down to restore heap property: Each level requires checking d children to find the smallest, so each comparison takes O(d) time. The maximum number of levels we might traverse is log_d(n), so the total work is O(d log_d n).

  **insert operation**: O(log_d n)
  - Add element as rightmost leaf: O(1)
  - Sift up to restore heap property: Each level requires 1 comparison with its parent. The maximum number of levels we might traverse is log_d(n), so the total work is O(log_d n).


- **1c.** In Dijkstra's algorithm using a d-ary heap:

  We perform |V| delete-min operations and at most |E| decrease-key (which involves at most one delete and one insert) operations.

  Work for delete-min operations: O(|V| · d log_d |V|)
  Work for decrease-key operations: O(|E| · log_d |V|)

  Total work: O(|V| · d log_d |V| + |E| · log_d |V|) = O((|V|d + |E|) log_d |V|)


- **1d.** Given |E| = |V|^(1+ε) for 0 < ε < 1, we want to find d such that the work is O(|E|).

  The work is O((|V|d + |E|) log_d |V|) = O((|V|d + |V|^(1+ε)) log_d |V|)
  
  Since |E| = |V|^(1+ε) dominates |V|d for reasonable values of d, we can simplify to:
  O(|V|^(1+ε) log_d |V|)

  We want this to equal O(|E|) = O(|V|^(1+ε))
  
  So we need log_d |V| = O(1)
  
  This happens when d = |V|^c for some constant c.
  
  Specifically, if we choose d = |V|^ε, then:
  log_d |V| = log |V| / log |V|^ε = 1/ε = O(1)
  
  And |V|d = |V| · |V|^ε = |V|^(1+ε) = |E|

  Therefore, choosing d = |V|^ε gives us a running time of O(|E|).


- **2a.** For the graph with 3 vertices shown in the image, let's compute APSP(i, j, k) for all i, j, k.

  APSP(i, j, k) represents the shortest path from i to j using only vertices {0, 1, ..., k}
  
  For k = 0 (only vertex 0 can be used as intermediate):
  - APSP(0, 0, 0) = 0 (path from 0 to 0 with no intermediate vertices)
  - APSP(0, 1, 0) = -2 (direct edge from 0 to 1)
  - APSP(0, 2, 0) = 2 (direct edge from 0 to 2)
  - APSP(1, 0, 0) = ∞ (no path from 1 to 0 using only vertex 0)
  - APSP(1, 1, 0) = 0 (path from 1 to 1 with no intermediate vertices)
  - APSP(1, 2, 0) = ∞ (no path from 1 to 2 using only vertex 0)
  - APSP(2, 0, 0) = ∞ (no path from 2 to 0 using only vertex 0)
  - APSP(2, 1, 0) = 1 (direct edge from 2 to 1)
  - APSP(2, 2, 0) = 0 (path from 2 to 2 with no intermediate vertices)

  For k = 1 (vertices 0 and 1 can be used as intermediates):
  - APSP(0, 0, 1) = 0 (same as before)
  - APSP(0, 1, 1) = -2 (direct edge from 0 to 1)
  - APSP(0, 2, 1) = 2 (direct edge from 0 to 2)
  - APSP(1, 0, 1) = ∞ (no path from 1 to 0 using only vertices 0 and 1)
  - APSP(1, 1, 1) = 0 (same as before)
  - APSP(1, 2, 1) = ∞ (no path from 1 to 2 using only vertices 0 and 1)
  - APSP(2, 0, 1) = ∞ (no path from 2 to 0 using only vertices 0 and 1)
  - APSP(2, 1, 1) = 1 (direct edge from 2 to 1)
  - APSP(2, 2, 1) = 0 (same as before)

  For k = 2 (all vertices 0, 1, and 2 can be used as intermediates):
  - APSP(0, 0, 2) = 0 (same as before)
  - APSP(0, 1, 2) = -2 (direct edge from 0 to 1)
  - APSP(0, 2, 2) = 2 (direct edge from 0 to 2)
  - APSP(1, 0, 2) = ∞ (still no path from 1 to 0)
  - APSP(1, 1, 2) = 0 (same as before)
  - APSP(1, 2, 2) = ∞ (no path from 1 to 2 in the graph)
  - APSP(2, 0, 2) = ∞ (no path from 2 to 0 in the graph)
  - APSP(2, 1, 2) = 1 (direct edge from 2 to 1)
  - APSP(2, 2, 2) = 0 (same as before)


- **2b.** Looking at the results from part 2a, I can observe that APSP(i, j, k) can be determined by considering:
  1. The direct path from i to j using only vertices 0 to k-1: APSP(i, j, k-1)
  2. The path that goes from i to k, then from k to j, using vertices 0 to k-1: APSP(i, k, k-1) + APSP(k, j, k-1)

  For example, APSP(i, j, 2) = min(APSP(i, j, 1), APSP(i, 2, 1) + APSP(2, j, 1))

  This relationship shows the optimal substructure property of the problem: the shortest path either doesn't use vertex k, or it can be broken down into shortest paths to and from vertex k.


- **2c.** Generalizing the observation from part 2b, we can derive the optimal substructure property for APSP(i, j, k):

  APSP(i, j, k) = min(APSP(i, j, k-1), APSP(i, k, k-1) + APSP(k, j, k-1))

  This formula states that the shortest path from i to j using vertices {0,1,...,k} is either:
  1. The shortest path from i to j using only vertices {0,1,...,k-1}, or
  2. The shortest path from i to k using vertices {0,1,...,k-1} followed by the shortest path from k to j using vertices {0,1,...,k-1}

  This is the key recurrence relation for the Floyd-Warshall algorithm.


- **2d.** Using top-down memoization, we need to compute APSP(i, j, k) for:
  - All source vertices i: |V| possibilities
  - All destination vertices j: |V| possibilities
  - All intermediate vertex sets k: |V| possibilities

  Total number of distinct subproblems: |V| × |V| × |V| = O(|V|³)

  For each subproblem, we do O(1) work (a simple min operation and addition). 

  Therefore, the total work of this dynamic programming algorithm is O(|V|³).


- **2e.** Comparing the work of our dynamic programming algorithm against Johnson's algorithm:

  - Dynamic Programming algorithm: O(|V|³)
  - Johnson's algorithm: O(|V||E| + |V|² log |V|)

  Our dynamic programming algorithm is preferable when:
  
  1. The graph is dense (|E| approaches |V|²)
     - In this case, Johnson's algorithm approaches O(|V|³)
     - Our DP algorithm has a better constant factor than Johnson's for dense graphs
  
  2. When the graph has negative edges but no negative cycles
     - Johnson's algorithm requires computing the re-weighting function, which adds overhead
     - Our DP algorithm naturally handles negative edges
  
  3. When simplicity of implementation is important
     - The Floyd-Warshall algorithm (which implements this DP approach) is conceptually simpler and easier to implement than Johnson's algorithm


- **3a.** No, a solution to the Minimum Spanning Tree (MST) problem is not guaranteed to be a solution to the Minimum Maximum Edge Tree (MMET) problem.

  The MST minimizes the sum of all edge weights, while the MMET minimizes the maximum edge weight in the spanning tree.

  Counter-example:
  Consider a graph with 4 vertices {A, B, C, D} and the following edges:
  - A-B: weight 5
  - B-C: weight 5
  - C-D: weight 5
  - D-A: weight 6
  - A-C: weight 10

  The MST would include edges A-B (5), B-C (5), and C-D (5), for a total weight of 15.
  
  The MMET could include edges D-A (6), A-B (5), and B-C (5). The maximum edge weight is 6.
  
  If we choose edges A-B (5), B-C (5), and C-D (5), the maximum edge weight is 5, which is lower than 6.
  
  Therefore, in this example, the MST solution (with maximum edge 5) is also the MMET solution, but this is not guaranteed in general. In other graphs, the MST could include a high-weight edge to minimize the total, while the MMET would avoid this edge even if it increases the total weight.


- **3b.** Algorithm to find the next best tree (after the optimal MST):

  1. Find the optimal MST T using Kruskal's or Prim's algorithm
  2. For each edge e in the optimal MST T:
     a. Remove e from the graph G
     b. Find the MST T' of the resulting graph (G - e)
     c. Add back edge e to G
     d. Record the total weight of T'
  3. Return the tree T' with the minimum total weight among all trees computed in step 2

  This algorithm works by considering alternatives to each edge in the optimal MST. When we remove an edge from the MST, we need to find the best replacement edge to form a new spanning tree. The "next best" tree will differ from the optimal MST by exactly one edge.


- **3c.** Work analysis of the algorithm:

  1. Finding the optimal MST using Kruskal's algorithm: O(|E| log |E|)
  2. For each edge in the MST (there are |V|-1 edges):
     a. Removing and adding back an edge: O(1)
     b. Finding the MST of the modified graph: O(|E| log |E|)
     c. Recording the weight: O(1)
  3. Finding the minimum weight tree among all candidates: O(|V|)
  
  Total work: O(|E| log |E| + |V| × |E| log |E|) = O(|V| × |E| log |E|)

  Since |V|-1 ≤ |E| in a connected graph, we can simplify this to O(|V| × |E| log |E|).

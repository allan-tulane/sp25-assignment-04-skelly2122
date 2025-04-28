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


- **2a.** For the graph with 3 vertices shown in the image, calculating APSP(i, j, k) for all i, j, k:

  From vertex 0:
  - APSP(0, 0, 0) = 0
  - APSP(0, 0, 1) = 0
  - APSP(0, 0, 2) = 0
  - APSP(0, 1, 0) = N/A
  - APSP(0, 1, 1) = -2
  - APSP(0, 1, 2) = -2
  - APSP(0, 2, 0) = N/A
  - APSP(0, 2, 1) = 2
  - APSP(0, 2, 2) = -1
  
  From vertex 1:
  - APSP(1, 0, 0) = N/A
  - APSP(1, 0, 1) = -2
  - APSP(1, 0, 2) = -2
  - APSP(1, 1, 0) = 0
  - APSP(1, 1, 1) = 0
  - APSP(1, 1, 2) = 0
  - APSP(1, 2, 0) = N/A
  - APSP(1, 2, 1) = 1
  - APSP(1, 2, 2) = 0
  
  From vertex 2:
  - APSP(2, 0, 0) = N/A
  - APSP(2, 0, 1) = 2
  - APSP(2, 0, 2) = -1
  - APSP(2, 1, 0) = N/A
  - APSP(2, 1, 1) = 1
  - APSP(2, 1, 2) = 0
  - APSP(2, 2, 0) = 0
  - APSP(2, 2, 1) = 0
  - APSP(2, 2, 2) = 0


- **2b.** Examining the calculated values, it's difficult to establish a consistent pattern from the data. Adding a new node doesn't necessarily guarantee a reduction in the shortest path length, which makes it challenging to formulate a definitive relationship between APSP(i, j, 1) and APSP(i, j, 2). Without a guaranteed improvement in path length when adding nodes, establishing a fixed relationship based solely on previous calculations isn't straightforward.


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


- **2e.** When comparing the computational efficiency of our dynamic programming approach against Johnson's algorithm:

  Our algorithm requires O(|V|³) work, while Johnson's algorithm has a complexity of O(|V||E|log(|E|)).
  
  This means our approach becomes preferable in situations where |E|log(|E|) > |V|², particularly in dense graph scenarios. In such cases, the dynamic programming method offers better performance characteristics than Johnson's algorithm, making it the more efficient choice for solving the all-pairs shortest path problem.


- **3a.** Yes, a solution to the Minimum Spanning Tree (MST) problem generally provides a solution to the Minimum Maximum Edge Tree (MMET) problem due to the lightest edge property. 

  The principle behind this is that if the MST weren't also the MMET for a given graph, it would imply the existence of an alternative tree with a lower maximum edge weight. However, this would contradict the fundamental definition of an MST, which constructs a spanning tree by consistently selecting the minimum weight edges available.
  
  The MST algorithm's greedy approach ensures that heavier edges are only included when absolutely necessary to maintain connectivity, thus naturally minimizing the maximum edge weight in the resulting tree.


- **3b.** To find the next best tree after determining the optimal MST, we begin with the initial MST and systematically explore alternative trees by considering all possible edge swaps. The process starts by identifying the original MST (T) and then examining every potential replacement edge from the set of available edges (|E|) to create alternative spanning trees. After evaluating all possible alternatives, we simply select the tree with the minimal total weight that isn't the original MST.


- **3c.** The computational complexity of this approach can be analyzed by examining each component:

  First, determining the initial MST requires O(|E| log |E|) work, as established in our class notes.
  
  The challenging part involves considering all potential edge swaps in the worst case, which could involve examining |E| different alternatives. Each alternative evaluation requires recalculating an MST, again at O(|E| log |E|) work.
  
  Combining these components yields a total computational complexity of O(|E| × |E| log |E|), which simplifies to O(|E|² log |E|) in the worst-case scenario.

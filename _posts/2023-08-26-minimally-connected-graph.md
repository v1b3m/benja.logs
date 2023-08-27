---
title: "Minimally Connected Graph"
categories:
  - dsa
tags:
  - facebook
  - graph
  - adjacency matrix
  - adjacency list
toc: true
---

## PROBLEM

This problem was asked by Facebook.
{: .notice--primary}



A graph is minimally-connected if it is connected and there is no edge that can be removed while still leaving the graph connected. For example, any binary tree is minimally-connected.

Given an undirected graph, check if the graph is minimally-connected. You can choose to represent the graph as either an adjacency matrix or adjacency list.

## SOLUTION

Alright! Let's break this problem down step by step.

### Algorithm

1. Check if the graph is connected.
2. If it is connected, then ensure that there are `n-1` edges in the graph where `n` is the number of nodes.
3. If both conditions are met, then it is minimally-connected.

Why does the second step ensure minimal connection? If we consider a tree structure (which is minimally connected by nature), a tree with `n` nodes will have `n-1` edges. If a graph has more than `n-1` edges and is connected, then there must be at least one edge that can be removed while still leaving the graph connected, hence it's not minimally connected.

Let's implement the above algorithm using the adjacency list representation, as it's more efficient for this task than the adjacency matrix.

### Typescript Implementation

```typescript
type Graph = Map<number, number[]>;

function isConnected(graph: Graph): boolean {
    const visited = new Set<number>();
    const startNode = Array.from(graph.keys())[0];

    // Depth First Search
    function dfs(node: number) {
        if (visited.has(node)) return;
        visited.add(node);
        for (let neighbor of graph.get(node)!) {
            dfs(neighbor);
        }
    }

    dfs(startNode);

    // All nodes must be visited for the graph to be connected
    return visited.size === graph.size;
}

function countEdges(graph: Graph): number {
    let edges = 0;
    for (let neighbors of graph.values()) {
        edges += neighbors.length;
    }
    return edges / 2;  // Since each edge is counted twice (once for each node)
}

function isMinimallyConnected(graph: Graph): boolean {
    return isConnected(graph) && countEdges(graph) === graph.size - 1;
}

// Example
const exampleGraph: Graph = new Map([
    [1, [2]],
    [2, [1, 3]],
    [3, [2]]
]);

console.log(isMinimallyConnected(exampleGraph));  // true
```

Here's what happens in the code:

1. `isConnected` function checks if the graph is connected using DFS.
2. `countEdges` function counts the number of edges in the graph.
3. `isMinimallyConnected` checks the conditions outlined in our algorithm.

The provided example is a simple graph with 3 nodes and 2 edges, which is minimally connected, so the function will return `true`.

## Q & A

### Why is any tree minimally connected by nature?

A tree is defined as a connected undirected graph with no cycles. Now, let's break down the reasoning behind why any tree is minimally connected by nature:

1. **Connectedness**: By definition, a tree is connected. This means there's a path between any pair of vertices.

2. **No Cycles**: The absence of cycles in a tree means that there's exactly one unique path between any two vertices. If there were more than one path between two vertices, we would have a cycle.

3. **Number of Edges**: A tree with \( n \) vertices has \( n - 1 \) edges. Why? This can be proven with induction:

   **Base case**: For \( n = 1 \), there are 0 edges.

   **Inductive step**: Assume a tree with \( k \) vertices has \( k - 1 \) edges. When we add one more vertex to this tree, we need to add exactly one edge to connect it to the tree (because the tree is connected by definition). So, a tree with \( k + 1 \) vertices has \( k - 1 + 1 = k \) edges.

Given the above properties:

- If we remove any vertex from a tree, the tree becomes disconnected.
- If we add an edge to the tree, we introduce a cycle.

Both of these changes break the definition of a tree.

Therefore, in a tree, there is no edge that can be removed without making the tree disconnected. This is the essence of minimal connectivity. Thus, any tree is minimally connected by nature.

### Why is the adjacency list solution more efficient than the adjacency matrix for this problem? What is an adjacency list anyway? And what is an adjacency matrix?

Both the adjacency list and adjacency matrix are representations of a graph, and each has its own advantages and disadvantages depending on the use-case. Let's understand both representations and why the adjacency list is more efficient for this problem:

#### 1. Adjacency Matrix

An adjacency matrix is a 2D array (usually a square matrix) that represents a graph. If `M[i][j]` is `1` (or some non-zero value), then there's an edge between vertex `i` and vertex `j`. If `M[i][j]` is `0`, then there's no edge between these vertices.

For a graph with `n` vertices:

- The adjacency matrix is of size `n x n`.
- Checking if there's an edge between two vertices is O(1).
- Space complexity is O(n^2).

Pros

- Constant-time operations to check the presence of an edge.
- Perfect for dense graphs where most of the vertex pairs have edges.

Cons

- Consumes more space for sparse graphs (where the number of edges is much less than `n^2`).
- Iterating over all edges or neighbors of a node takes O(n), even if a vertex has only one neighbor.

#### 2. Adjacency List

An adjacency list represents a graph as a list (or array or map) where each vertex has its own list (or array) of neighbors. 

For a graph with `n` vertices and `e` edges:

- There are `n` lists (one for each vertex).
- The total number of entries across all lists is `2e` (since each edge is counted twice: once for each vertex).
- Checking if there's an edge between two vertices can take up to O(n) in the worst case.
- Space complexity is O(n + e).

Pros

- Consumes less space for sparse graphs.
- Direct access to all neighbors of a vertex.
- Space complexity is linear with respect to the size of the graph.

Cons

- Checking for the presence of an edge between two vertices can take longer compared to an adjacency matrix.

#### Efficiency for this Problem:

For the problem of checking if a graph is minimally connected:

1. We need to traverse the entire graph to ensure it's connected.
2. We also need to count the number of edges.

With an adjacency matrix:

- Counting all the edges requires going through all `n^2` entries of the matrix, even if the graph is sparse.
  
With an adjacency list:

- We can traverse the entire graph and count the edges by simply going through each vertex's neighbors, resulting in a complexity directly proportional to `n + e`.

Given that we do not know if the graph is dense or sparse, and considering that counting edges in a sparse graph via an adjacency matrix would be highly inefficient, the adjacency list representation is generally more efficient for this problem.

## Resources

1. [Connected Graph](https://www.sciencedirect.com/topics/mathematics/connected-graph)

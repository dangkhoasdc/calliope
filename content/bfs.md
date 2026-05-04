# Breadth-First Search

## Identifying problems

- Graph or search problems: each state is a node and all edges (connections) have uniform (or none) weights.
- Need to consider which state need to add to the queue (the `Matrix 01` problem).

## Strategies

- Almost always need to maintain a visited set to avoid visiting the same node multiple times.
- To further optimize the search, immediately when we reach the target, we return the result instead of putting it in the queue and continue searching.
This is especially useful when we are looking for the shortest path.

## Problems

- [Perfect Squares](https://leetcode.com/problems/perfect-squares/)
- [Open The Lock](https://leetcode.com/problems/open-the-lock/)
- [Number Of Islands](https://leetcode.com/problems/number-of-islands/)
- [Matrix 01](https://leetcode.com/problems/01-matrix)

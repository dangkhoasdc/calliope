---
title: "Topological Sort: A Permutation Perspective"
date: 2026-02-16
tags:
    - sorting
---

## Introduction
Topological sort is a classic algorithm problem: given a directed acyclic graph (DAG), produce a linear ordering of vertices such that every edge points from an earlier vertex to a later one. You encounter this pattern everywhere:

- **Build systems** like [Bazel](https://github.com/bazelbuild/bazel/blob/26230734e5a4c4450ab68f0159ac28b614f4f3d9/src/main/java/com/google/devtools/build/lib/graph/Digraph.java#L527) must compile source files in dependency order
- **Package managers** like [pip](https://github.com/pypa/pip/blob/fa81791e4add3395e0dffc3e253a2c7da1efe68d/src/pip/_internal/resolution/resolvelib/resolver.py#L218) install libraries only after their dependencies are ready
- **Task schedulers** like [Celery](https://github.com/celery/celery/blob/67b328abd7d1cbafe7393081609552aab3948204/celery/utils/graph.py#L63) sequence jobs where some tasks must complete before others begin
- **Neural network training** computes gradients in [reverse topological order](https://gist.github.com/karpathy/8627fe009c40f57531cb18360106ce95#file-microgpt-py-L56-L67) during backpropagation

Most textbooks present topological sort through Kahn's algorithm or depth-first search.
But there is another way to think about it: as a search through the space of permutations.
It reveals why topological sort admits multiple valid solutions, connects the problem to constraint satisfaction, and leads naturally to algorithms that enumerate all possible orderings.

Think of topological sorting as arranging all vertices in a line such that all edges point forward.

> Instead of asking "how do I traverse the graph?", we ask "which permutations of vertices satisfy the edge constraints?"

Given $n$ vertices, there are $n!$ possible arrangements, but only a subset respects the edge directions.
A DAG with many edges has few valid orderings; a DAG with few edges has many.

Consider a simple chain A → B → C → D.
Only one permutation works: [A, B, C, D].
But if we remove the edge B → C, suddenly [A, C, B, D] becomes valid too.
Each edge is a constraint that eliminates permutations.

This explains why topological sort typically has multiple solutions.
Unless the graph forms a single chain, the constraints leave room for choice.
When you run Kahn's algorithm or DFS, you get one valid ordering, but others exist.

## Formulation

Given a DAG $G = (V, E)$, a topological ordering is a bijection $\pi: V \to \{1, 2, \ldots, n\}$ such that:

$$\forall (u, v) \in E: \pi(u) < \pi(v)$$

Here $\pi(x)$ gives the position of vertex $x$ in the ordering.
The constraint says: for every edge from $u$ to $v$, vertex $u$ must appear before vertex $v$.

This makes the problem's structure clear.
We have $n!$ possible bijections, and each edge $(u, v)$ eliminates roughly half of them (those where $v$ precedes $u$).
The valid topological orderings are exactly the bijections that survive all edge constraints.

## Implementation

The permutation perspective suggests a natural algorithm: build the ordering by swapping elements into place, only allowing vertices whose predecessors are already positioned.

```python
def all_topological_sorts(graph):
    n = len(graph)
    # Build predecessor list for quick lookup
    pred = [[u for u in range(n) if v in graph[u]] for v in range(n)]
    cur = list(range(n))  # initial permutation
    result = []

    def backtrack(i):
        if i == n:
            result.append(cur.copy())
            return
        # Try every vertex from i to n-1 as the next element
        for j in range(i, n):
            v = cur[j]
            # Check if all predecessors of v are already placed
            if all(p in cur[:i] for p in pred[v]):
                cur[i], cur[j] = cur[j], cur[i]
                backtrack(i + 1)
                cur[i], cur[j] = cur[j], cur[i]

    backtrack(0)
    return result
```

At each recursion level `i`, we have already fixed positions `0` through `i-1`.
We scan the remaining vertices and check which ones are eligible: a vertex qualifies only if all its predecessors already appear in the fixed prefix.
When we find an eligible vertex, we swap it into position `i`, recurse, then swap back to try other candidates.
Note that `all([]) == True`, so vertices with no predecessors are always eligible at the start.

This is the standard permutation-generation algorithm with one addition: the eligibility check that prunes invalid branches early.
The swapping technique avoids allocating new lists at each level, keeping the implementation minimal.

## Complexity

The worst-case complexity depends on how many valid topological orderings exist.
For a graph with no edges, every permutation is valid, giving $n!$ orderings.
The algorithm must enumerate all of them, so the worst case is $O(n! \cdot n)$ where the extra $n$ factor comes from the eligibility check at each level.

In practice, edges prune the search tree significantly. A densely connected DAG might have only one valid ordering, making the algorithm run in $O(n^2)$ time.
The actual performance falls somewhere between these extremes, determined by the graph's structure.

For finding just one topological ordering, Kahn's algorithm or DFS both run in $O(V + E)$ time.
The backtracking approach is slower but serves a different purpose: it finds all valid orderings, which is inherently expensive when many exist.

## Further Reading
The permutation-based approach to topological sorting is discussed in *The Art of Computer Programming*, Volume 4A [@knuth_taocp4a] (section 7.2.1.2) and *Recursion via Pascal* [@rohl_recursion] (chapter 7).

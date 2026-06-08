---
title: "Depth-First Search"
---
# Strategies
Or how to properly implement DFS:

1. Avoid revisiting nodes by marking them. Sets are usually a good choice for this; otherwise if node identifier is an integer, a boolean array can be used.
2. Returning value of the recursive call.
3. Most of the time, we need to maintain some global state to keep track of the result.

## Graph diameter

```python
def calc_diameter(graph):
    if len(graph) == 0: return 0

    diameter = 0
    heights = [0] * len(graph)

    def dfs(cur, parent):
        max1, max2 = 0, 0
        for child in graph[cur]:
            if child == parent: continue
            dfs(child, cur)
            heights[cur] = max(heights[cur], heights[child])

            if heights[child] >= max1:
                max2 = max1
                max1 = heights[child]
            elif heights[child] > max2:
                max2 = heights[child]
        heights[cur] += 1
        nonlocal diameter
        diameter = max(diameter, heights[cur], max1 + max2 + 1)

    dfs(0, -1)
    return diameter-1
```

# Problems

- [Clone Graph](https://leetcode.com/problems/clone-graph/description/)

# Template

# Related Algorithms

- [Eulerian Path](graphs/euler_path.md)
- [Cycle Detection](graphs/cycle.md)

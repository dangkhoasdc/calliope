# Depth-First Search

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

## Problems

- [Clone Graph](https://leetcode.com/problems/clone-graph/description/)

## Template

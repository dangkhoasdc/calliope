---
title: "Topological Sort"
date: 2026-05-12
---

# Kahn's Algorithm

The idea is straightforward: repeatedly find vertices with no incoming edges, add them to the ordering, and remove them from the graph.

```python
g = defaultdict(list)
in_degree = [0] * n
for u, v in edges:
    g[u].append(v)
    in_degree[v] += 1

q = deque([i for i in range(n) if in_degree[i] == 0])
topo_order = []
while q:
    u = q.popleft()
    topo_order.append(u)
    for v in g[u]:
        in_degree[v] -= 1
        if in_degree[v] == 0:
            q.append(v)
return topo_order if len(topo_order) == n else []  # return empty if cycle detected
```

# Applications

- Finding tree centers.
- Scheduling tasks with dependencies.

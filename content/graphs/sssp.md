---
title: "Single Source Shortest Path"
tags:
    - graphs
---

# Edge Relaxation

The name is confusing to me, it should be named "path relaxation" instead.
Let say we have a path from `A-B` with distance `d`, then later we find a better route from `A-B`, e.g., through `C` with distance `d'` such as `d' < d`.
We update our knowledge of the shortest path from `A-B` to `d'`.

# Dijkstra's Algorithm

We need to keep track:
- Shortest path from source to the current vertex, i.e, `(dist, cur_vert)`.
- Whenever we discover a shorter path to the current, we broadcast the new path to all of its neighbors,
and update their shortest path if the new path is shorter.

# Bellman-Ford Algorithm

Outline:

```python
queue = Deque([(src, 0)])
for _ in range(V):
    for _ in range(len(queue)):
        city, cur_price = queue.popleft()
        # go to the neighbor
        for next_city, price in edges[city]:
            new_price = cur_price + price
            if new_price < dist[next_city]:
                dist[next_city] = new_price
                queue.append((next_city, new_price))
```

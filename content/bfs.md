# Breadth-First Search

## Identifying problems

- Graph or search problems: each state is a node and all edges (connections) have uniform (or none) weights.
- Need to consider which state need to add to the queue (the `Matrix 01` problem).

## Template

```py
def bfs(...):
    # handle edge case, early returns
    # most of the time it is the seed value pushed to the queue

    # Init
    queue = Deque()
    visited = ...

    """
    Add a new item to the queue.
    Additional step includes checking whether the item is already visited.
    """
    def add_to_queue(new_item):
        ...

    """
    Find all valid neighbors given the current item and add them to the queue.
    Optimization: early termination by checking the stop condition on the neighbor
    """
    def neighbors(item):
        ...

    def update_queue(item):
        for neighbor in neighbors(item):
            add_to_queue(neighbor)

    queue.append(seed)
    stop_condition = False
    while stop_condition and len(queue) > 0:
        for _ in range(len(queue)):
            cur_item = queue.popleft()
            if check(value):
                stop_condition = True
                break
            update_queue(cur_item)
```

## Problems

- [Perfect Squares](https://leetcode.com/problems/perfect-squares/)
- [Open The Lock](https://leetcode.com/problems/open-the-lock/)
- [Number Of Islands](https://leetcode.com/problems/number-of-islands/)
- [Matrix 01](https://leetcode.com/problems/01-matrix)

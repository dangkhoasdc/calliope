# Heap & Priority Queues

## Data Structures

### Basic

### Python

`heapq` provides a min heap.

```Python
import heapq as pq

heap = []
item = None

pq.heapify(heap)

min_item = pq.heappop(heap)
pq.heappush(heap, item)

```

- `heapify` is `O(N)`, so use it instead of gradually putting items into the heap via `push` (`O(Nlg N)`).

## Patterns

- Most likely related to the greedy approach.
- The need to maintain a statistic, and we want to quickly access the value without re-computing from the input.

## Problems

- [Maximum Average Pass Ratio](https://leetcode.com/problems/maximum-average-pass-ratio/description/): This is the fun one since we need to figure out what *statistic* we want to access.

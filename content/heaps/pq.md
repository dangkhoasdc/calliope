---
title: "Heaps"
---
# Priority Queues

One maybe confused about the difference between a heap and a priority queue.
A priority queue is an abstract data type that supports the following operations:

1. Insert.
2. Remove the "max" element.

Here, "max" can be interpreted as an **extreme** element which can be determined by some comparison functions.

Heap, on the other hand, is a specific data structure that can be used to implement a priority queue.

Queues and stacks are in fact special cases of priority queues where the "max" element is determined by the order of insertion.
Specifically, in a queue, the "max" element is the one that was inserted first (FIFO), while in a stack, the "max" element is the one that was inserted last (LIFO).


# Heap

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

- Most likely related to the greedy approach while at each step we want to access the "max" element (or "min" element) in the current state of the input.
- The need to maintain a statistic or ordering, and we want to quickly access the value without re-computing from the input.

## Problems

- [Maximum Average Pass Ratio](https://leetcode.com/problems/maximum-average-pass-ratio/description/): This is the fun one since we need to figure out what _statistic_ we want to access.

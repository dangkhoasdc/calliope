# Array

## Traversal

- Sum of index is a constant when we travel on the diagonal lines: `i + j = C`.

### Problems

- [Diagonal Traverse](https://leetcode.com/problems/diagonal-traverse/)

## In-situ permutation

Solve pretty much all problems related to array rearrangement using O(1) extra memory.

### Reference

- Section 7.8, Sedgewick, Robert, and Philippe Flajolet. An introduction to the analysis of algorithms. Addison-Wesley Longman Publishing Co., Inc., 1996.
- Exercise 10-13, 5.2 Internal Sorting. TAOCP Vol 3.

## Two Pointer Techniques

### Applications

- Array Partition.
- Exchange-based sorting algorithms (bubble sort, quicksort).
- Insertion sort.

# Problems

- [Dutch national flag problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem)
- [Valid Mountain Array](https://leetcode.com/problems/valid-mountain-array)
- [Max Consecutive Ones II](https://leetcode.com/problems/max-consecutive-ones-ii)
- [Move Zeroes](https://leetcode.com/problems/move-zeroes)
- [Image Smoother](https://leetcode.com/problems/image-smoother/)

## Sliding Windows

### Patterns

- Problems related to counting the number of subarray satisfying some conditions.

To count number of subarray inside the range `[left, right]`, number of subarrays ending at `right`:

```
cnt = right - left + 1
```

- May need a dict to hold some statistics of the subarray instead of re-computing for each subarray (normally takes `O(N^3)`).

## Problems

- [Continuous Subarrays](https://leetcode.com/problems/continuous-subarrays/description/)

## Related Topics

- [Prefix Sum](./ps.md)

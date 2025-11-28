# Binary Search

## Template

```python
def binary_search(lower: int, upper: int, f: Callable) -> int:
    lo, hi = lower, upper
    
    while lo < hi:
        mi = (lo + hi) // 2
        if f(mi):
            hi = mi
        else:
            lo = mi + 1
    return lo
```


N.B.: It is [almost identical](https://github.com/python/cpython/blob/f7d1109a1227dec99c6fb08c7e64aecc7acafa02/Lib/bisect.py#L21)
to the `bisect_right` function from the `bisect` module.
`lo` is the smallest element satisfying the `f` condition, i.e., the first `x` from left to right of the input array:

```
[o o o o o x x x x x x x]
```

The most important aspect of binary search problem is to find the **monotonic objective** encoded in the problem.

Next, we need identify the `lower` and `upper` bounds of the search space.

## Patterns

- The problem asks to find minimum (or maximum) value.
- The problems requires `log(N)` time complexity.
- The input is sorted.


## Practices

- [Minimum Limit of Balls in a Bag](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag)
- [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/).
- [H-Index](https://leetcode.com/problems/h-index-ii)
- [Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/)
- [Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements)

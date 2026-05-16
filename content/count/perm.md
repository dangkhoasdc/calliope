---
title: Permutations
date: 2026-05-14
---

# Swap and Backtrack

The idea is simple: we gradually fix elements from left to right, then recursively permute the remaining elements.
After we explore a permutation, we choose another element for the rightmost position of the fixed part, and repeat the process.
This *choosing* step can be done by swapping the current element with its right.

In the implementation, after the recursive call, we need to swap back to restore the original order for the next iteration.

```python
def permute(self, nums: List[int]) -> List[List[int]]:
    n = len(nums)
    res = []
    def backtrack(i):
        if i == n:
            res.append(list(nums))
            return
        for j in range(i, n):
            nums[i], nums[j] = nums[j], nums[i]
            backtrack(i+1)
            nums[i], nums[j] = nums[j], nums[i]
    backtrack(0)
    return res
```

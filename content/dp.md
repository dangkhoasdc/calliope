# Dynamic Programming

## Recursion

### Forward Recursion

I saw a common pattern where thinking forwardly is easier to reason. For example, the [House Robber](https://leetcode.com/problems/house-robber/description/) problem:

```python
from functools import cache

def rob(self, nums: List[int]) -> int:
    n = len(nums)
    @cache
    def dp(i: int) -> int:
        if i >= n: return 0
        return max(
            nums[i] + dp(i+2),  # rob the current house, skip the next one
            dp(i+1)             # skip the current house, rob the next one
        )

    return dp(0)
```

The logic is simple: at the ith house, we either: (1) rob it and move to the next next house (`i+2`), or (2) we don't rob and move to the next one. When we go over the nth house, we rob nothing (`0`) since there is no more house to rob.

[This problem](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/description/) also has the similar approach:

```python
from functools import cache

def maximumScore(self, nums: List[int], multipliers: List[int]) -> int:
    n, m = len(nums), len(multipliers)
    @cache
    def dp(i, left):
        # inclusive [left, right] of nums
        if i >= m: return 0
        right = n - 1 - (i - left)
        return max(
            nums[left] * multipliers[i] + dp(i+1, left+1),
            nums[right] * multipliers[i] + dp(i+1, left)
        )
    return dp(0, 0)
```

### Backward Recursion

- Or backtracking.
- Noticeably, we need to define the _base case_.

### Background

## Strategies

### 1. Finding state variables

### 2. Defining the recursion relation

- Simplify the basecase.
- ALWAYS VERIFY THE BASE CASE.

### 3. Finalizing the top-down approach

- Using Python, if the recursion relation is found, then it is pretty much done with top-down approach (via `cache` in `functools`).

### 4. Selecting data structures for memorization

### 5. Finding the computation path

### 6. Finalizing the bottom-up approach

## Other optimizations

### Optimized Bottom-Up DP

### Monotonic Stacks

- [Minimum Difficulty of a Job Schedule](https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/description/)

#### Problems

## Patterns

## Problems

- [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence)
- [Delete and Earn](https://leetcode.com/problems/delete-and-earn/description/)
- [Maximal Square](./mrp.md)
- [Minimum Difficulty of a Job Schedule](https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/description/)
- [Coin Change](https://leetcode.com/problems/coin-change/description/)

## Reading

- [Leetcode's Dynamic Programming](https://leetcode.com/explore/learn/card/dynamic-programming/): a great place to start practice DP.

Codeforces contains many insightful tutorials on DP:

- [Everything About Dynamic Programming](https://codeforces.com/blog/entry/43256)
- [Advanced Tricks](https://codeforces.com/blog/entry/47764)

Series from `rembocoder` is worth checking out:

- [Dynamic Programming: Prologue](https://codeforces.com/blog/entry/102542)
- [How to solve DP problems: Regular Approach](https://codeforces.com/blog/entry/102178)
- [Two Kinds of Dynamic Programming: Process Simulation](https://codeforces.com/blog/entry/100918)
- [Simple DP optimizations](https://codeforces.com/blog/entry/104798)

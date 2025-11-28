# Binary Search

## Scaffolding
1. Find the search domain, i.e., the close interval containing all valid values. In other words, all posibilities that the optimal solution can be.
2. Define the **objective**.
3. Sort the search domain, make them monotinic to certain property (most of the time, it is the value of **objective**).
4. Apply binary search.

## Tips

- (Python), don't rely on the `bisect` module from the library, write your own binary search.
- Most of the time, the problems requires a little bit of tweak on binary search.


## The textbook implementation

```python
def binsearch(lst, key):
    low, high = 0, len(lst) - 1 
    while low <= high:
        mid = (low + high) // 2
        if lst[mid] == key: return mid
        elif lst[mid] < key: low = mid + 1 
        else: high = mid - 1 
    return -1  # NOT FOUND
```

## Variant 1: 2-condition binary search 

```python
def binsearch_tc(lst, cnd):
    low, high = 0, len(lst) - 1
    while low < high:
        mid = (low + high) // 2
        if cond(i):
            high = mid
        else: low = mid + 1
    return low
```

## Problems

- [My List on Leetcode](https://leetcode.com/list?selectedList=rxo6ycwr)
- [Find Peak Element](https://leetcode.com/problems/find-peak-element/)
- [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array)


# References
- Column 4, Writing Correct Programs. Programming Pearls: A must read for anyone who wants to master binary search.
- Chapter 6.2.1, Searching an Ordered Table, TAOCP Vol3.
- Chapter 6.2, Binary Search and Its Variances, Introduction to Algorithms - A create approach.

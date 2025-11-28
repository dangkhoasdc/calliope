# Stacks

## Nested Objects

## Nearest Smaller Values

```python
stack = []
res = []
for item in collection:
    while stack and stack[-1] >= item:
        # pop recent items that is larger than the current one
        stack.pop()

    if stack: # this is the nearest smaller item (to the left)
        res.append(stack[-1])
    else: # there is no such item
        res.append(None)

    stack.append(item)
```

### Properties

- The stack is sorted in the ascending order for finding the nearest smaller values.
Hence, [Binary Search](bs.md) can be applied on the stack to quickly retrieve the value sastifying a condition.

### Problems

- [Final Prices With a Special Discount in a Shop](https://leetcode.com/problems/final-prices-with-a-special-discount-in-a-shop/description/)
- [Max Chunks To Make Sorted](https://leetcode.com/problems/max-chunks-to-make-sorted/description/)
- [Find Building Where Alice and Bob Can Meet](https://leetcode.com/problems/find-building-where-alice-and-bob-can-meet/description/)

## Related topics

- [Depth First Search](dfs.md)

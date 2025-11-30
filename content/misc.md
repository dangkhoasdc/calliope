# Misc

## Why Python?

- Less verbose than C++ & Java, concise syntax looks almost like pseudocode.
- Support wide range of data structures, e.g., priority queues via `heap`, Deque via `deque`, graph via `defaultdict`.

## Patterns

- Nested loop: use `product` from `itertools`:

```python
for i, j in product(range(N), range(N)):
    ...
```

- Memorization: Use `cache` in `functools`, almost always required for DP problems (unless you tabulate it).

## Links

- [https://www.jjinux.com/2022/08/python-my-favorite-python-tricks-for.html](https://www.jjinux.com/2022/08/python-my-favorite-python-tricks-for.html)

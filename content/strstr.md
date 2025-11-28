# strstr
## Problem
Retuning the index of the first occurrence of `needle` string in `haystack` string, or `-1` if there is no occurrence.
## Brute Force Approach

Andrei has [a thread on Twitter](https://x.com/incomputable/status/1664347304399126528) about `strstr`, his favorite interview question.
His solution:

```cpp
const char* mystrstr(const char* haystack, const char* needle) {
    const char* h = haystack;
    const char* n = needle;

    for (;;) {
        if (!*n) return haystack;
    ehm:
        if (!*h) return nullptr;
        if (*n++ != *h++) {
            h = ++haystack;
            n = needle;
            goto ehm;
        }
    }
}
```

It is concise, cool and use some intriguing optimization:

- Increment operators make 5 operators (2 dereferences, 1 comparison, and 2 increments) compresses into just 1 LOC.
- `goto` helps remove the redundant check of `needle` every time we see a new mismatched character.

This is my Python implementation, one can use [this problem on LeetCode](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string) to test the correctness:

::: lora.stralgs.str_str.bf_1

Python does not use NULL-terminated string, so we need to get the length of each strings to determine when to stop. Plus, additional optimization via `goto` can not be replicated in Python^[To me, `goto` is just cool, imperative PLs should always have it].

An even smaller version uses `for-else`:

::: lora.stralgs.str_str.bf_2

![Brute Force Benchmark](/assets/images/strstr/bf.svg)


## References
- Charras, Christian, and Thierry Lecroq. "Handbook of exact string matching algorithms." (2004).

---
layout: "post.njk"
title: "Some Quicksort Variants"
date: 2018-01-04
tags:
    - sortings
---

Let's begin with a quick refresher on quicksort. Here's a straightforward implementation of this celebrated algorithm:

```cpp
// quicksort implement
template<typename T>
int partition(vector<T>& a, int lo, int hi) {
    int i = lo, j = hi;
    T v = a[lo];
    while (true) {
        while (a[++i]<=v) if (i == hi) break;
        while (v<=a[--j]) if (j == lo) break;
        if (i >= j)  break;
        swap(a[i], a[j]);
    }
    swap(a[lo], a[j]);
    return j;
}

template<typename T>
void quicksort(vector<T>& a, int lo, int hi) {
    if (lo >= hi-1) return;
    int j = partition(a, lo, hi);

    quicksort(a, lo, j);
    quicksort(a, j+1, hi);
}
```

### Analysis

The complexity is evaluated through the function:

$$ C(N) = n-1 + {1\over n}\sum_{i=0}^{n-1} ( C(i) + C(n-i-1)) $$

During partitioning, $n-1$ comparisons occur. Quicksort then recurses on subarrays of sizes $i$ and $n-i-1$. Since each element is equally likely to be the pivot (probability ${1\over n}$), we arrive at the expression above.

The solution technique appears in classics like *Introduction to Algorithms* [@cormen_intro], *Concrete Mathematics* [@conmath], and *Introduction to Analysis of Algorithms* [@sedgewick_analysis]. In brief:

1. Multiply both sides by $n$.
2. Write the expression for $n-1$.
3. Calculate $C(n) - C(n-1)$ to get a linear recurrence relation, which can be solved.

## Limiting the Worst-case

A celebrated technique to sidestep quicksort's worst-case behavior is shuffling the input array beforehand.
The [Knuth shuffle](https://en.wikipedia.org/wiki/Fisher-Yates_shuffle), running in $\mathcal{O}(N)$ time, guarantees the algorithm's expected efficiency.
This enhancement—often called randomized quicksort—has become a textbook example in the analysis of randomized algorithms.
I discuss the analysis of randomized quicksort in a [separate post](/vi/randalgs).

>[!caution]
> Link above is in Vietnamese.

Beyond shuffling, several other strategies exist:

* Rather than selecting the pivot from the beginning or end, choose the median value.
* When all values are identical—another worst-case scenario—3-way quicksort comes to the rescue.

## Removing Recursion for Small Arrays

This optimization applies to any divide-and-conquer algorithm: when the recursion reaches subarrays below a threshold (typically 10–15 elements), switching to `insertion sort` yields significant performance gains. Simply add this check to the quicksort function:

```cpp
///...
if (hi-lo < CUTOFF) {
 insertion_sort(a, lo, hi);
 return;
}
///...
```

## Eliminating Boundary Value Comparisons

With the implementation above, you can eliminate the boundary checks inside the `while` loops:

* The check for `j` is unnecessary because `a[lo] < a[lo]` never holds.
* To remove the check for `i`, place a sentinel value like `MAX_INT` at the array's end before calling quicksort. This guarantees an element greater than `a[lo]` always exists.

## Median-of-three and Median-of-five

A time-tested technique to avoid worst-case behavior: sample 3 or 5 elements, compute their median, and use it as the pivot.

### Tukey ninther quicksort

Use 9 elements instead of 3/5.

## 3way Quicksort

This celebrated algorithm, proposed by Dijkstra alongside the [Dutch national flag problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem), handles arrays with many duplicate values gracefully. Remarkably, this variant achieves entropy-optimal performance—a brief proof appears in Sedgewick's *Algorithms* [@sedgewick_algs].

If you're wondering why Sedgewick features so prominently in this post, his PhD thesis bears the title: "[Quicksort](http://searchworks.stanford.edu/view/870273)".

Here's an elegant version by Jon Bentley (author of *Programming Pearls* [@bentley_pearls], who collaborated with Sedgewick on the entropy-optimality proof):

```c
void quicksort(int l, int u){
    int i, m;
    if(l >= u) return;
    m = l;
    for(i = l+1; i<= u; i++)
        if(x[i] < x[l])
            swap(++m, i); //this is where I'm lost. Why the heck does it preincrement?

    swap(l, m);

    quicksort(l, m-1);
    quicksort(m+1, u);
}
```

## Non-recursive Version

Another Sedgewick improvement replaces recursion with an explicit stack. A thorough C implementation appears [here](http://www.cs.columbia.edu/~hgs/teaching/isp/hw/qsort.c). This version incorporates the enhancements discussed above:

* CUTOFF
* Median-of-three
* Non-recursive and optimized stack size.

This is also the version of `qsort` in GNU C.

Sedgewick's Java version is also worth exploring: [source code QuickX](https://algs4.cs.princeton.edu/23quicksort/QuickX.java.html).

## Samplesort

This parallel variant of quicksort distributes work across multiple processes.
The key insight: use quicksort to identify pivots for each process, then run quicksort independently on each partition.
[This article](http://users.atw.hu/parallelcomp/ch09lev1sec5.html) describes the algorithm and analyzes its complexity.

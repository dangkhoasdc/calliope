---
title: "Disjoint Sets"
---

# Overview

Disjoint sets (or Union-Find) is quite practical.
In fact, during the time working for product search development at Visenze, I implemented it several times for grouping relevant products.

Basically, whenever we need to manage and control a **set** of equivalent groups and do not care much about individual item, Disjoint Sets are really good.
In addition, the optimal version of it is also easy to implement.

Other practical usecases:
- Detect if graph contains a cycle.
- Kruskal's algorithm for [[graphs/mst.md]]

# Mechanism

## Interface

```d
// find the root of the current node
int find(int cur);
```

## Optimal Disjoint Sets

Our data structure maintains the parent (or root) of each node and several relavant info such as how many equivalent groups it is holding at the moment.
In addition, we also keep track of the heights (or rank) of each tree representation of the equivalent group.

Everytime we conduct root finding, we also optimizes the associated tree by reducing the height/rank of it simply by assigning the parent to the root of the tree.

When combining two groups, we attach the shorter tree to the larger one so that there is no change in their rank.
To break tie, we arbitrarily assign one parent to other, increasing the rank of that other one by 1.

## Other variants

In many other usecases, one can modify the weight to other statistics that are more relevant to the problem (e.g., [this puzzle](https://leetcode.com/problems/evaluate-division/))
When the number of distinct item is unknown, one can use map/dictionary to maintain the parent.

Interestingly, in most cases, the updating to the root node (i.e., the group index), or other optimization is conducted in the `find` operation instead of the `union` operation.

If the vertex has it own weight, to use this data structure, one can create a virtual vertex and attach the vertex weight to the edge between the virtual vertex and the original vertex.
This technique is useful with problems related to minimum spanning trees.

Usefule materials:
- [Wiki](https://en.wikipedia.org/wiki/Disjoint-set_data_structure)
- [Algorithms by Sedgewick](https://algs4.cs.princeton.edu/15uf/): the best material so far.
- Knuth also mentioned this data structure somewhere in TAOCP chapter 2.

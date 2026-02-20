---
title: "Binary Search Trees"
---

# Definition

```d
struct Node {
    int key;
    Node* left;
    Node* right;
}
```

# Validation

My first attemp is to use the current node as the reference point,
and check if the left and right nodes are less than and greater than the current node, respectively.
However, this approach fails for certain cases, such as when the left child of a node has a key that is greater than the current node's key.
This also complicates the interface as we need to propagate the maximum and minimum of the subtree up to the parent node,
which can be cumbersome and error-prone.

A better approach is to use a range `[min, max]` to validate the current node's key,
and then recursively validate the left and right subtrees with updated ranges.
When we move to the left subtree, we update the maximum to be the current node's key while keeping the minimum unchanged.
Same for the right subtree but we swap the roles of minimum and maximum.

The overall strategy here is to determine the how the recursion function propagates the necessary information
without needing to return complex data structures or maintain additional state outside of the recursive calls.

```d
bool validate(Node* root, int min=int.min, int max=int.max) {
    if (root == null) return true;
    if (root->key <= min || root->key >= max) return false;
    return validate(root->left, min, root->key) && validate(root->right, root->key, max);
}
```

---
title: Traversal in Binary Trees
---

# Finding successor of node in inorder traversal

```d
Node* succ(Node* root, Node* target) {
    res = null;
    while (root != null) {
        if (root->key > target->key) {
            res = root;
            root = root->left;
        } else {
            root = root->right;
        }
    }
    return res
}
```

- When `root->key > target->key`: The current node is a candidate successor, so we save it.
But there might be a smaller node that's still greater than target in the left subtree, so we go left to find a potentially better (smaller) successor.
- When `root->key <= target->key`: The current node cannot be the successor, so we go right to find larger nodes.

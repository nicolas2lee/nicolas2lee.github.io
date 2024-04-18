---
title: Interview binary tree
date: 2024-04-06
description: Interview binary tree 
tag: interview, binary tree, algorithm
author: nicolas2lee
---

# Interview binary tree
## Lowest Common Ancestor of a Binary Tree (LCA)
### LCA for ordinary binary tree
#### Problem
[Leetcode 236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)
#### Solution 1
From root, dfs to find the node p & q, and record path. Once found the path to p & to q,
backtrack to find the least common parent of both paths.

#### Solution 2
If a node is LCA of p and q, then the left subtree should contain p,
and right subtree should contain q
```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root==null || root.val==p.val || root.val == q.val) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left!=null && right !=null) return root;
        return left!=null ? left : right;
    }
```
### LCA for binary search tree
#### Problem
[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)
#### Solution 
If value of node p,q less than current node, then p,q are in left subtree in current node.
If value of node p,q greater than current node, then p,q are in right subtree in current node.
If value of current node is between p, q, then current node is the lca

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root==null || root.val == p.val || root.val == q.val) return root;
        if (root.val < p.val && root.val < q.val) return lowestCommonAncestor(root.right, p, q);
        if (root.val > p.val && root.val > q.val) return lowestCommonAncestor(root.left, p, q);
        return root;
    }
}
```



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
<details>
  <summary>Show code</summary>
  
```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root==null || root.val==p.val || root.val == q.val) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left!=null && right !=null) return root;
        return left!=null ? left : right;
    }
```

</details>

### LCA for binary search tree
#### Problem
[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)
#### Solution 
If value of node p,q less than current node, then p,q are in left subtree in current node.
If value of node p,q greater than current node, then p,q are in right subtree in current node.
If value of current node is between p, q, then current node is the lca

<details>
  <summary>Show code</summary>
  
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

</details>

## Lowest Common Ancestor of a Binary Tree (LCA)
### Delete Nodes And Return Forest
#### Problem
[1110. Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/description/)
#### Solution
Let's define a helper function ```delete(node)```, and use post order to visit the tree.
If cur node to delete, then add left & right child into roots, return null to the parent node, so the parent node will point to null (it means no subtree)
If cur node should be deleted, then no change.

<details>
  <summary>Show code</summary>
    
```java
class Solution {
    List<TreeNode> roots;

    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        roots = new ArrayList<>();
        var set = new HashSet<Integer>();
        for(var x: to_delete) set.add(x);
        var node = delete(root, set);
        if (node!=null) roots.add(node);
        return roots;
    }

    TreeNode delete(TreeNode cur, Set<Integer> toDeleteSet){
        if (cur==null) return null;
        var deleted = toDeleteSet.contains(cur.val);
        cur.left = delete(cur.left, toDeleteSet);
        cur.right = delete(cur.right, toDeleteSet);
        if (deleted){
            if (cur.left!=null) roots.add(cur.left);
            if (cur.right!=null) roots.add(cur.right);
            return null;
        }else{
            return cur;
        }
    }
}
```
</details>

## Find leaves of binary tree
### Delete Nodes And Return Forest
#### Problem
[366. Find Leaves of Binary Tree](https://leetcode.com/problems/find-leaves-of-binary-tree/description/)

Given the root of a binary tree, collect a tree's nodes as if you were doing this:
* Collect all the leaf nodes.
* Remove all the leaf nodes.
* Repeat until the tree is empty.

Example:
```
Input: root = [1,2,3,4,5]
Output: [[4,5,3],[2],[1]]
```

#### Solution
##### Solution 1 (optimal)
Iterate the whole binary tree, ths position of each node in the result is the height of the node starting from the buttom,
leaf node has height 0 and root node has max height

<details>
  <summary>Show code</summary>
    
```java
class Solution {
    private List<List<Integer>> solution;
    
    private int getHeight(TreeNode root) {
        // return -1 for null nodes
        if (root == null) {
            return -1;
        }
        // first calculate the height of the left and right children
        int leftHeight = getHeight(root.left);
        int rightHeight = getHeight(root.right);
        int currHeight = Math.max(leftHeight, rightHeight) + 1;
        if (this.solution.size() == currHeight) {
            this.solution.add(new ArrayList<>());
        }
        this.solution.get(currHeight).add(root.val);
        return currHeight;
    }
    
    public List<List<Integer>> findLeaves(TreeNode root) {
        this.solution = new ArrayList<>();
        getHeight(root);
        return this.solution;
    }
}
```
</details>

##### Solution 2 (topological sort)
Consider the binary tree as a graph, the edge is uni directional starting from buttom

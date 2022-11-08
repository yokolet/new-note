---
layout: post
title: Lowest Common Ancestor of a Binary Search Tree
hero_height: is-small
tags:
- Medium
- Depth-First Search
- Binary Search Tree
date: 2022-10-29 14:02 +0900
---
## Introduction
This is a popular LCA problem of the binary search tree (BST).
The binary tree's LCS is [Lowest Common Ancestor of a Binary Tree](/algo/trees/2022-09-26-lowest-common-ancestor-of-a-binary-tree).
Either of DFS or BFS works to find the solution.
Compare to the binary tree, the BST can find the answer quickly in general.
That's because BST can check the node value and decide which subtree to go deeper.
However, assuming the BST is balanced, we can say BST can find the answer quickly.

## Problem Description
> Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.
>
> According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): "The lowest 
> common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants
> (where we allow a node to be a descendant of itself)."
>
> Constraints:
> - The number of nodes in the tree is in the range `[2, 10**5]`.
> - `-10**9 <= Node.val <= 10**9`
> - All Node.val are unique.
> - `p != q`
> - `p` and `q` will exist in the BST.
>
> [https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

## Examples
```
Example 1
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
      6
    /   \
  2      8
 / \    / \
0   4  7   9
   / \
  3   5
```

```
Example 2
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
      6
    /   \
  2      8
 / \    / \
0   4  7   9
   / \
  3   5
```

```
Example 3
Input: root = [2,1], p = 2, q = 1
Output: 2
```

## Analysis
It is a BST, so if the root value lies between p and q node values, it is a lca.
If both p and q values are smaller than root value, it should check left subtree.
Otherwise, go right subtree.

## Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class LowestCommonAncestorOfABinarySearchTree:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root or root == p or root == q or\
            p.val < root.val < q.val or q.val < root.val < p.val:
            return root
        if p.val < root.val and q.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
        else:
            return self.lowestCommonAncestor(root.right, p, q)
```

## Complexities
- Time: `O(n)` -- if the BST is well balanced, this can be log(n)
- Space: `O(n)`

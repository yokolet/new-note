---
layout: post
title: Inorder Successor in BST
hero_height: is-small
tags:
- Medium
- Depth-First Search
- Binary Search Tree
date: 2022-09-30 14:23 +0900
---
## Introduction
If the tree is the binary search tree (BST), the inorder successor has next bigger value.
Basic idea is the same as the binary tree inorder traversal.
However, using BST's property, we can go to one of left or right subtree instead of both.

## Problem Description
> Given the `root` of a binary search tree and a node p in it, return the
> in-order successor of that node in the BST.
> If the given node has no in-order successor in the tree, return null.
>
> The successor of a node `p` is the node with the smallest key greater than `p.val`.
>
> Constraints:
> - The number of nodes in the tree is in the range `[1, 10**4]`.
> - `-10**5 <= Node.val <= 10**5`
> - All Nodes will have unique values.
>
> [https://leetcode.com/problems/inorder-successor-in-bst/](https://leetcode.com/problems/inorder-successor-in-bst/)

## Examples
```
Example 1
Input: root = [2,1,3], p = 1
Output: 2
Explanation:
   2
 /   \
1     3
1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.
```

```
Example 2
Input: root = [5,3,6,2,4,null,null,1], p = 6
Output: null
Explanation:
       5
     /   \
    3     6
   / \
  2   4
 /
1
There is no in-order successor of the current node, so the answer is null.
```

## Analysis
Since the tree is BST, compare the value to go which subtree.
If the root node's value is greater than the given node, go left.
Otherwise, go right.
The successor is the smallest in the right subtree or parent if the node is a left child.
If the node is a right child and doesn't have the right subtree, no successor exists.

## Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class InorderSuccessorInBST:
    def inorderSuccessor(self, root: TreeNode, p: TreeNode) -> Optional[TreeNode]:
        if not root or not p: return None
        if root.val > p.val:
            return self.inorderSuccessor(root.left, p) or root
        return self.inorderSuccessor(root.right, p)
```

## Complexities
- Time: `O(n)`  -- worst case. if bst is balanced, it will be O(log(n))
- Space: `O(n)`

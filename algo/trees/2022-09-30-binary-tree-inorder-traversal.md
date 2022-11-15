---
layout: post
title: Binary Tree Inorder Traversal
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Depth-First Search
- Binary Tree
date: 2022-09-30 14:05 +0900
---
## Introduction
This is a basic inorder traversal problem.
Just visit all nodes in the binary tree by the inorder manner.

## Problem Description
> Given the `root` of a binary tree, return the inorder traversal of its nodes' values.
>
> Constraints:
> - The number of nodes in the tree is in the range `[0, 100]`.
> - `-100 <= Node.val <= 100`
>
> [https://leetcode.com/problems/binary-tree-inorder-traversal/](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Examples
```
Example 1
Input: root = [1,null,2,3]
Output: [1,3,2]
Explanation:
1
 \
  2
 /
3
```

```
Example 2
Input: root = []
Output: []
```

```
Example 3
Input: root = [1]
Output: [1]
```

## Analysis
The tree traversal takes the depth-first search (DFS) style.
The helper function does the DFS and saves values between going left and right.

## Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class BinaryTreeInorderTraversal:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def helper(root, result):
            if not root:
                return result
            result = helper(root.left, result)
            result.append(root.val)
            result = helper(root.right, result)
            return result
        return helper(root, [])
```

## Complexities
- Time: `O(n)`
- Space: `O(n)`

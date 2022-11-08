---
layout: post
title: Binary Search Tree Iterator
hero_height: is-small
tags:
- Medium
- Binary Search Tree
- Design
- Iterator
date: 2022-09-30 14:59 +0900
---
## Introduction
The idea is the same as BST's inorder traversal.
However, it needs additional data structure to save parent nodes since there's no way to traverse back to the root.
Other than that, the next bigger value in BST is the node's leftmost in right subtree if the node has right child.
If the node is the left child, the parent is the successor. Otherwise, no successor.

## Problem Description
> Implement the `BSTIterator` class that represents an iterator over the in-order traversal of a binary search tree (BST):
> - `BSTIterator(TreeNode root)` Initializes an object of the `BSTIterator` class.
>    The `root` of the BST is given as part of the constructor. The pointer should be
>    initialized to a non-existent number smaller than any element in the BST.
> - `boolean hasNext()` Returns true if there exists a number in the traversal to the right of the pointer,
>    otherwise returns false.
> - `int next()` Moves the pointer to the right, then returns the number at the pointer.
>
> Notice that by initializing the pointer to a non-existent smallest number, the first call to `next()` will return
> the smallest element in the BST.
>
> You may assume that `next()` calls will always be valid. That is, there will be at least a next number
> in the in-order traversal when `next()` is called.
>
> Constraints:
> - The number of nodes in the tree is in the range `[1, 10**5]`.
> - `0 <= Node.val <= 10**6`
> - At most 10**5 calls will be made to `hasNext`, and `next`.
>
> [https://leetcode.com/problems/binary-search-tree-iterator/](https://leetcode.com/problems/binary-search-tree-iterator/)

## Examples
```
Example 1
   7
 /   \
3     15
     /  \
    9    20
Input
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
Output
[null, 3, 7, true, 9, true, 15, true, 20, false]
```

## Analysis
The solution uses a stack to save parents since the binary tree doesn't have a way to go back to its parent.
Also, the solution defines an utility method, find_next. This method finds the successor.
The next method implementation uses find_next utility method starting from a current node's right child.
The hasNext method checks the stack size which represents a number of parents of left child.

## Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class BSTIterator:

    def __init__(self, root: Optional[TreeNode]):
        self.stack = []
        self.find_next(root)

    def find_next(self, root: Optional[TreeNode]):
        while root:
            self.stack.append(root)
            root = root.left

    def next(self) -> int:
        cur = self.stack.pop()
        if cur.right:
            self.find_next(cur.right)
        return cur.val
        
    def hasNext(self) -> bool:
        return self.stack
```

## Complexities
- Time: `O(n)`
- Space: `O(n)`

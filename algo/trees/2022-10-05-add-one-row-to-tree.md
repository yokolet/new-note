---
layout: post
title: Add One Row to Tree
hero_height: is-small
tags:
- Medium
- Breadth-First Search
- Depth-First Search
- Binary Tree
date: 2022-10-05 14:13 +0900
---
## Introduction
Both of depth-first search with/without stack and the breadth-first search work.
Once the level to add new nodes is found, just create two new nodes and set those in the tree.

## Problem Description
> Given the `root` of a binary tree and two integers `val` and `depth`, add a row of nodes with value `val` at
> the given depth `depth`. Note that the root node is at depth 1.
>
> The adding rule is:
> - Given the integer `depth`, for each not null tree node `cur` at the depth `depth - 1`, create two tree nodes
>   with value `val` as cur's left subtree root and right subtree root.
> - `cur`'s original left subtree should be the left subtree of the new left subtree root.
> - `cur`'s original right subtree should be the right subtree of the new right subtree root.
> - If `depth == 1` that means there is no depth `depth - 1` at all, then create a tree node with value `val`
>   as the new root of the whole original tree, and the original tree is the new root's left subtree.
>
> Constraints:
> - The number of nodes in the tree is in the range `[1, 10**4]`.
> - The depth of the tree is in the range `[1, 10**4]`.
> - `-100 <= Node.val <= 100`
> - `-10**5 <= val <= 10**5`
> - `1 <= depth <= the depth of tree + 1`
>
> [https://leetcode.com/problems/add-one-row-to-tree/](https://leetcode.com/problems/add-one-row-to-tree/)

## Examples
```
Example 1
Input: root = [4,2,6,3,1,5], val = 1, depth = 2
Output: [4,1,1,2,null,null,6,3,1,5]
Explanation:
Input:
      4
    /   \
  2      6
 / \    /
3   1   5

Output:
       4
     /   \
    1     1
   /       \
  2        6
 / \      /
3   1    5
```

```
Example 2
Input: root = [4,2,null,3,1], val = 1, depth = 3
Output: [4,2,null,1,1,3,null,null,1]
Explanation:
Input:
     4
    /
  2
 / \
3   1

Output:
      4
     /
    2
   /  \
  1    1
 /      \
3        1
```

## Analysis
As the problem describes, only when depth is 1, it needs a special treatment.
The solution taken here is the breadth-first search approach and level order traversal.
In each level, the depth is decremented.
When the depth becomes 1, create two new nodes and add those to the current parent.
The new nodes' children are current parent's children.

## Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class AddOneRowToTree:
    def addOneRow(self, root: Optional[TreeNode], val: int, depth: int) -> Optional[TreeNode]:
        if depth == 1:
            return TreeNode(val=val, left=root)
        queue = [root]
        while queue:
            q_len = len(queue)
            depth -= 1
            for _ in range(q_len):
                cur = queue.pop(0)
                if depth == 1:
                    left = TreeNode(val=val, left=cur.left)
                    right = TreeNode(val=val, right=cur.right)
                    cur.left,cur.right = left, right
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            if depth == 1:
                break
        return root
```

## Complexities
- Time: `O(n)`
- Space: `O(k)`  -- k: number of the nodes in the same level

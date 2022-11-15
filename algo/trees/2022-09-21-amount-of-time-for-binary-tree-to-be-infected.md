---
layout: post
title: Amount of Time for Binary Tree to Be Infected
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Breadth-First Search
- Depth-First Search
- Binary Tree
date: 2022-09-21 15:48 +0900
---
## Introduction
Among a couple of possible solutions, the easiest would be to create a graph first.
Then, the problem comes down to a simple graph traversal.

## Problem Description
> You are given the `root` of a binary tree with unique values, and an integer `start`.
> At minute 0, an infection starts from the node with value start.
> Each minute, a node becomes infected if:
> - The node is currently uninfected.
> - The node is adjacent to an infected node.
>
> Return the number of minutes needed for the entire tree to be infected.
>
> Constraints:
> - The number of nodes in the tree is in the range `[1, 10**5]`.
> - `1 <= Node.val <= 10**5`
> - Each node has a unique value.
> - A node with a value of `start` exists in the tree.
>
> [https://leetcode.com/problems/amount-of-time-for-binary-tree-to-be-infected/](https://leetcode.com/problems/amount-of-time-for-binary-tree-to-be-infected/)

## Examples
```
Example 1
Input: root = [1,5,3,null,4,10,6,9,2], start = 3
Output: 4
Explanation:
      1
    /   \
 5       [3]
  \      / \
   4    10  6
  /  \
 3    2
- Minute 0: Node 3
- Minute 1: Nodes 1, 10 and 6
- Minute 2: Node 5
- Minute 3: Node 4
- Minute 4: Nodes 9 and 2
```

```
Example 2
Input: root = [1], start = 1
Output: 0
```

## Analysis
The first step is to create a graph by traversing the binary tree.
Since the value is unique, node's value can be used as a node id.
Then, traverse the graph from the given start value with time 0.
This solution uses the breadth-first search.
When the next node is popped, compare the maximum time.
Go over adjacent nodes in the graph.
The node is not yet visited, add it to the queue with incremented time.

## Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class AmountOfTimeForBinaryTreeToBeInfected:
    def amountOfTime(self, root: Optional[TreeNode], start: int) -> int:
        graph = collections.defaultdict(set)
        
        def helper(root, parent):
            if not root:
                return
            if parent:
                graph[root.val].add(parent.val)
                graph[parent.val].add(root.val)
            helper(root.left, root)
            helper(root.right, root)
        
        helper(root, None)
        queue, seen = [(start, 0)], set()
        max_v = 0
        while queue:
            v, t = queue.pop(0)
            seen.add(v)
            max_v = max(max_v, t)
            for next_v in graph[v]:
                if next_v not in seen:
                    queue.append((next_v, t + 1))
        return max_v
```

## Complexities
- Time: `O(n)`
- Space: `O(n + h)` -- h: height of the three
 

---
layout: post
title: Delete Node in a Linked List
hero_height: is-small
tags:
- Medium
- Linked List
date: 2022-10-13 17:19 +0900
---
## Introduction
If it comes to the linked list problems, deleting a node is a common one.
Normally, the head of the linked list is given, so the solution in general traverses the list.
However, this problem doesn't give us the head, so we are unable to traverse the list.
This problem tests our idea. Once it is figured out, the solution is fairly easy.

## Problem Description
> There is a singly-linked list `head` and we want to delete a node `node` in it.
> You are given the node to be deleted `node`. You will not be given access to the first node of `head`.
> All the values of the linked list are unique, and it is guaranteed that the given node `node` is not the last
> node in the linked list.
>
> Delete the given node. Note that by deleting the node, we do not mean removing it from memory. We mean:
> - The value of the given node should not exist in the linked list.
> - The number of nodes in the linked list should decrease by one.
> - All the values before node should be in the same order.
> - All the values after node should be in the same order.
>
> Constraints:
> - The number of the nodes in the given list is in the range `[2, 1000]`.
> - `-1000 <= Node.val <= 1000`
> - The value of each node in the list is unique.
> - The node to be deleted is in the list and is not a tail node.
>
> [https://leetcode.com/problems/delete-node-in-a-linked-list/](https://leetcode.com/problems/delete-node-in-a-linked-list/)

## Examples
```
Example 1
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5,
the linked list should become 4 -> 1 -> 9 after calling your function.
```

```
Example 2
Input: head = [4,5,1,9], node = 1
Output: [4,5,9]
Explanation: You are given the third node with value 1,
the linked list should become 4 -> 5 -> 9 after calling your function.
```

## Analysis
This problem doesn't give us the head of the linked list.
We have just a node to be deleted.
However, it is not a doubly linked list, so it's impossible to delete the given node.
Instead, the next node of the given node can be deleted easily.
Copy the value of the next node to the given node, then delete the next node.
The result will be as if the given node is deleted.

## Solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class DeleteNodeInALinkedList:
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        node.val = node.next.val
        node.next = node.next.next
```

## Complexities
- Time: `O(1)`
- Space: `O(1)`

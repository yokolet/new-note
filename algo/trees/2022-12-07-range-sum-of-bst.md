---
layout: post
title: Range Sum of BST
algo_menubar: algo_menu
hero_height: is-small
tags: []
date: 2022-12-07 16:20 +0900
---
## Problem Description
> Given the `root` node of a binary search tree and two integers low and high, return the sum of values of all nodes
> with a value in the inclusive range `[low, high]`.
>
> Constraints:
> - The number of nodes in the tree is in the range `[1, 2 * 10**4]`.
> - `1 <= Node.val <= 10**5`
> - `1 <= low <= high <= 10**5`
> - All Node.val are unique.
>
> [https://leetcode.com/problems/range-sum-of-bst/](https://leetcode.com/problems/range-sum-of-bst/)

## Examples
```
Example 1
Input: root = [10,5,15,3,7,null,18], low = 7, high = 15
Output: 32
Explanation: Nodes 7, 10, and 15 are in the range [7, 15]. 7 + 10 + 15 = 32.
      10
    /    \
  5       15
 / \        \
3   7        18
```

```
Example 2
Input: root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
Output: 23
Explanation: Nodes 6, 7, and 10 are in the range [6, 10]. 6 + 7 + 10 = 23.
        10
      /    \
    5      15
   / \    /  \
  3   7  13  18
 /   /
1   6
```

## How to Solve
This problem can be solved in multiple ways such that the breadth-first search, depth-first search, post-order or
in-order traversal.
The solution here took the breadth-first search approach.
Since the tree is BST, a direction to go left or right depends on the root value.
When the root value is between low and high, it goes both directions.
If the root value is greater than high, it goes to left.
If the root value is less than low, it goes to right.
As the problem describes, if the root value is between low and high inclusive, add it up to the total.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class RangeSumOfBST {
public:
    int rangeSumBST(TreeNode* root, int low, int high) {
        queue<TreeNode*> q({root});
        int total = 0;
        while (!q.empty()) {
            TreeNode *cur = q.front();
            q.pop();
            if (low <= cur->val && cur->val <= high) {
                total += cur->val;
                if (cur->left) {
                    q.push(cur->left);
                }
                if (cur->right) {
                    q.push(cur->right);
                }
            } else if (high < cur->val && cur->left) {
                q.push(cur->left);
            } else if (cur->val < low && cur->right) {
                q.push(cur->right);
            }
        }
        return total;
    }
};
```
{% endtab %}

{% tab solution Java %}
```java

```
{% endtab %}

{% tab solution JavaScript %}
```js

```
{% endtab %}

{% tab solution Python %}
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class RangeSumOfBST:
    def rangeSumBST(self, root: Optional[TreeNode], low: int, high: int) -> int:
        queue = [root]
        total = 0
        while queue:
            cur = queue.pop(0)
            if low <= cur.val <= high:
                total += cur.val
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            elif high < cur.val and cur.left:
                queue.append(cur.left)
            elif cur.val < low and cur.right:
                queue.append(cur.right)
        return total
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(n)`
- Space: `O(n)`

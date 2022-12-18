---
layout: post
title: Find Leaves of Binary Tree
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Depth-First Search
- Binary Tree
date: 2022-12-18 16:53 +0900
---
## Problem Description
> Given the `root` of a binary tree, collect a tree's nodes as if you were doing this:
> - Collect all the leaf nodes.
> - Remove all the leaf nodes.
> - Repeat until the tree is empty.
>
> Constraints:
> - The number of nodes in the tree is in the range `[1, 100]`.
> - `-100 <= Node.val <= 100`
>
> [https://leetcode.com/problems/find-leaves-of-binary-tree/](https://leetcode.com/problems/find-leaves-of-binary-tree/)

## Examples
```
Example 1
Input: root = [1,2,3,4,5]
Output: [[4,5,3],[2],[1]]
Explanation:
[[3,5,4],[2],[1]] and [[3,4,5],[2],[1]] are also considered correct answers since per each level it does not matter
the order on which elements are returned.
       1
     /   \
   2      3
 /   \
4     5
```

```
Example 2
Input: root = [1]
Output: [[1]]
```

## How to Solve
This problem asks tree heights to leaf nodes.
The postorder traversal (depth-first search) works well.
Traverse the tree to the leaf node, which will have the level 0.
When it comes back from the lower node, calculate the level as `1 + max(left, right)`.
The level works as the index of the result array.
If the result array is shorter than the level, append an empty array.
Insert the root value to the array of the level index.

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
class Solution {
private:
    int traverse(TreeNode *root, vector<vector<int>> &levels) {
        if (!root) { return -1; }
        int left = traverse(root->left, levels);
        int right = traverse(root->right, levels);
        int level = 1 + max(left, right);
        if (levels.size() < level + 1) {
            levels.push_back({});
        }
        levels[level].push_back(root->val);
        return level;
    }
public:
    vector<vector<int>> findLeaves(TreeNode* root) {
        vector<vector<int>> levels;
        traverse(root, levels);
        return levels;
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
class FindLeavesOfBinaryTree:
    def findLeaves(self, root: Optional[TreeNode]) -> List[List[int]]:
        levels = []

        def traverse(root):
            if not root:
                return -1
            left = traverse(root.left)
            right = traverse(root.right)
            level = 1 + max(left, right)
            if len(levels) < level + 1:
                levels.append([])
            levels[level].append(root.val)
            return level

        traverse(root)
        return levels
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(n)`  -- n: the number of tree nodes
- Space: `O(h)` -- h: the height of a given tree

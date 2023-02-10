---
layout: post
title: Binary Tree Maximum Path Sum
algo_menubar: algo_menu
hero_height: is-small
tags:
- Hard
- Depth-Frist Search
- Binary Tree
date: 2022-12-10 15:06 +0900
---
## Problem Description
> A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge
> connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass
> through the root.
>
> The path sum of a path is the sum of the node's values in the path.
>
> Given the `root` of a binary tree, return the maximum path sum of any non-empty path.
>
>
> Constraints:
> - The number of nodes in the tree is in the range `[1, 3 * 10**4]`.
> - `-1000 <= Node.val <= 1000`
>
> [https://leetcode.com/problems/binary-tree-maximum-path-sum/](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Examples
```
Example 1
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
   1
 /   \
2     3
```

```
Example 2
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
    -10
  /     \
9       20
       /  \
     15    7
```

## How to Solve
Do postorder (DFS) traversal and calculate the value, The node value + left subtree value + right subtree value,
which is the sum at the current root.
Compare the value with the max value so far.
When returning the value, considerable paths are: root + left subtree, root + right subtree, or 0 (doesn't take
this subtree). Return the max value of those three.

What needs to be care about is, how to save max value so far. Python solution pass it as a method argument
and returns it. Ruby solution uses an instance variable. C++ solution passes a reference.

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
class BinaryTreeMaximumPathSum {
public:
    int traverse(TreeNode *root, int &max_v)
    {
        if (!root)
            return 0;
        int left = traverse(root->left, max_v);
        int right = traverse(root->right, max_v);
        max_v = max(max_v, root->val + left + right);
        return max(root->val + max(left, right), 0);
    }
    
    int maxPathSum(TreeNode* root) {
        int max_v = INT_MIN;
        traverse(root, max_v);
        return max_v;
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
class BinaryTreeMaximumPathSum:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
      def traverse(root: Optional[TreeNode], max_v: int):
        if not root:
          return max_v, 0
        max_v, left = traverse(root.left, max_v)
        max_v, right = traverse(root.right, max_v)
        max_v = max(max_v, root.val + left + right)
        return max_v, max(root.val + max(left, right), 0)

      max_v = float('-inf')
      max_v, _ = traverse(root, max_v)
      return max_v
```
{% endtab %}

{% tab solution Ruby %}
```ruby
# Definition for a binary tree node.
# class TreeNode
#     attr_accessor :val, :left, :right
#     def initialize(val = 0, left = nil, right = nil)
#         @val = val
#         @left = left
#         @right = right
#     end
# end
# @param {TreeNode} root
# @return {Integer}
def max_path_sum(root)
    @max_value = -Float::INFINITY
    traverse = -> (root) {
      return 0 if root.nil?
      l_max = [traverse.call(root.left), 0].max
      r_max = [traverse.call(root.right), 0].max
      @max_value = [@max_value, root.val + l_max + r_max].max
      return [root.val, root.val + [l_max, r_max].max].max
    }
    traverse.call(root)
    @max_value
end
```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(n)`
- Space: `O(h)` -- h: height of the binary tree

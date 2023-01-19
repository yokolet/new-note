---
layout: post
title: Construct Binary Search Tree from Preorder Traversal
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Divide and Conquer
- Binary Search Tree
date: 2023-01-17 17:22 +0900
---
## Problem Description
> Given an array of integers preorder, which represents the preorder traversal of a BST (i.e., binary search tree),
> construct the tree and return its root.
>
> It is guaranteed that there is always possible to find a binary search tree with the given requirements for the given
> test cases.
>
> A binary search tree is a binary tree where for every node, any descendant of `Node.left` has a value strictly less
> than `Node.val`, and any descendant of `Node.right` has a value strictly greater than `Node.val`.
>
> A preorder traversal of a binary tree displays the value of the node first, then traverses `Node.left`, then
> traverses `Node.right`.
>
> Constraints:
> - `1 <= preorder.length <= 100`
> - `1 <= preorder[i] <= 1000`
> - All the values of `preorder` are unique.
>
> [https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

## Examples
```
Example 1
Input: preorder = [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]
```

```
Example 2
Input: preorder = [1,3]
Output: [1,null,3]
```

## How to Solve
A couple of approaches are there to solve this problem such as recursion, iterative, with inorder, etc.
Among those, the divide and conquer (recursion) would be an easy one.
The recursion approach might use indices or lower/upper bound values.
The solution here uses the divide and conquer with indices.

The preorder array's start index will be the current node's value.
The left subtree will be up to the index where the value is smaller than the current node.
The right subtree will be the preorder array's greater values than current node.
After the recursion, BST will be created.

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
class ConstructBinarySearchTreeFromPreorderTraversal {
private:
    TreeNode* helper(vector<int> &preorder, int start, int end){
        if (start > end) { return nullptr; }
        TreeNode* root = new TreeNode(preorder[start]);
        int idx = start + 1; 
        while (idx <= end && preorder[idx] < preorder[start]) {
            idx++;
        }
        root -> left = helper(preorder, start + 1, idx - 1);
        root -> right = helper(preorder, idx, end);
        return root;
    }
public:
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        return helper(preorder, 0, preorder.size() - 1);
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
class ConstructBinarySearchTreeFromPreorderTraversal:
    def bstFromPreorder(self, preorder: List[int]) -> Optional[TreeNode]:
        def helper(start, end):
            if start > end:
                return None
            root = TreeNode(preorder[start])
            idx = start + 1
            while idx <= end and preorder[idx] < preorder[start]:
                idx += 1
            root.left = helper(start + 1, idx - 1)
            root.right = helper(idx, end)
            return root
        return helper(0, len(preorder) - 1)
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(n)`
- Space: `O(h)` -- h: height of the tree

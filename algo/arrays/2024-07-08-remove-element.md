---
layout: post
title: Remove Element
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Array
- Two Pointers
date: 2024-07-08 15:07 +0900
---
## Problem Description
> Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` in-place. The order
> of the elements may be changed. Then return the number of elements in `nums` which are not equal to `val`.
>
> Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to
> do the following things:
>
> - Change the array `nums` such that the first `k` elements of `nums` contain the elements
>   which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
> - Return k.
>
> Constraints:
> - `0 <= nums.length <= 100`
> - `0 <= nums[i] <= 50`
> - `0 <= val <= 100`
>
> [https://leetcode.com/problems/remove-element/](https://leetcode.com/problems/remove-element/)

## Examples
```
Example 1
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

```
Example 2
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores)
```

## How to Solve

Two-pointers are a good approach here. Since swapping values is not required, values other than `val` should be
put in the nums one by one. One pointer goes one by one, while another pointer increments only when the value
is not `val`.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class RemoveElement {
public:
    int removeElement(vector<int>& nums, int val) {
        int idx = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] != val) {
                nums[idx] = nums[i];
                idx++;
            }
        }
        return idx;
    }
};
```
{% endtab %}

{% tab solution Java %}
```java
class RemoveElement {
    public int removeElement(int[] nums, int val) {
        int idx = 0;
        for (int i = 0; i < nums.length; ++i) {
          if (nums[i] != val) {
            nums[idx] = nums[i];
            idx++;
          }
        }
        return idx;
    }
}
```
{% endtab %}

{% tab solution JavaScript %}
```js
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    let idx = 0;
    for (let i = 0; i < nums.length; ++i) {
      if (nums[i] !== val) {
        nums[idx] = nums[i]
        idx++;
      }
    }
    return idx;
};
```
{% endtab %}

{% tab solution Python %}
```python

```
{% endtab %}

{% tab solution Ruby %}
```ruby
# @param {Integer[]} nums
# @param {Integer} val
# @return {Integer}
def remove_element(nums, val)
    idx = 0
    0.upto(nums.size - 1) do |i|
      if (nums[i] != val)
        nums[idx] = nums[i]
        idx += 1
      end
    end
    idx
end
```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(n)`
- Space: `O(1)`

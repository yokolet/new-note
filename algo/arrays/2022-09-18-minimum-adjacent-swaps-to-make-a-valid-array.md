---
layout: post
title: Minimum Adjacent Swaps to Make a Valid Array
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Array
- Greedy
date: 2022-09-18 21:20 +0900
---
## Introduction
This is a minimum swaps to rearrange something problem.
This would be an easier problem compared to other problems of this kind.
The leftmost smallest and rightmost largest values move.
Just looking at indices of those two values will lead to the answer.

## Problem Description
> You are given a 0-indexed integer array `nums`.
> Swaps of adjacent elements are able to be performed on nums.
> A valid array meets the following conditions:
> - The largest element (any of the largest elements if there are multiple) is
>    at the rightmost position in the array.
> - The smallest element (any of the smallest elements if there are multiple) is
>    at the leftmost position in the array.
>
> Return the minimum swaps required to make nums a valid array.
>
> Constraints:
> - `1 <= nums.length <= 10**5`
> - `1 <= nums[i] <= 10**5`
>
> [https://leetcode.com/problems/minimum-adjacent-swaps-to-make-a-valid-array/](https://leetcode.com/problems/minimum-adjacent-swaps-to-make-a-valid-array/)

## Examples
```
Example 1
Input: nums = [3,4,5,5,3,1]
Output: 6
Explanation: Perform the following swaps:
- Swap 1: Swap the 3rd and 4th elements, nums is then [3,4,5,[3,5],1]. 
- Swap 2: Swap the 4th and 5th elements, nums is then [3,4,5,3,[1,5]].
- Swap 3: Swap the 3rd and 4th elements, nums is then [3,4,5,1,3,5].
- Swap 4: Swap the 2nd and 3rd elements, nums is then [3,4,1,5,3,5].
- Swap 5: Swap the 1st and 2nd elements, nums is then [3,1,4,5,3,5].
- Swap 6: Swap the 0th and 1st elements, nums is then [1,3,4,5,3,5].
It can be shown that 6 swaps is the minimum swaps required to make a valid array.
```

```
Example 2
Input: nums = [9]
Output: 0
Explanation: The array is already valid, so we return 0.
```

## Analysis
Find the leftmost smallest value's index and rightmost largest value's index.
The smallest value's swap count is the index of that value.
The largest value's swap count is (n - 1 - largest value's index)
or (n - 2 - largest value's index) depends on the smallest and largest indices.
If smallest values index is bigger than largest index, a swap for the largest is
done once. That's why n - 1 or n - 2.

## Solution
```python
class MinimumAdjacentSwapsToMakeAValidArray:
    def minimumSwaps(self, nums: List[int]) -> int:
        n = len(nums)
        smallest, largest = (float('inf'), -1), (float('-inf'), -1)
        for idx, v in enumerate(nums):
            if smallest[0] > v:
                smallest = (v, idx)
            if largest[0] <= v:
                largest = (v, idx)
        if smallest[0] == largest[0]:
            return 0
        s_idx, l_idx = smallest[1], largest[1]
        if s_idx < l_idx:
            return s_idx + n - 1 - l_idx
        else:
            return s_idx + n - 2 - l_idx
```

## Complexities
- Time: `O(n)`
- Space: `O(1)`
 

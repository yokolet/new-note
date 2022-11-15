---
layout: post
title: Largest Perimeter Triangle
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Math
- Sorting
- Greedy
date: 2022-10-20 15:12 +0900
---
## Introduction
This problem requires a bit of trigonometry knowledge.
To form a triangle, 3 sides a, b, c should have the length, a + b > c, where c is the longest side.
Sort the given array, then find 3 largest values which satisfy the condition.

## Problem Description
> Given an integer array `nums`, return the largest perimeter of a triangle with a non-zero area, formed from three of
> these lengths. If it is impossible to form any triangle of a non-zero area, return 0.
>
> Constraints:
> - `3 <= nums.length <= 10**4`
> - `1 <= nums[i] <= 10**6`
>
> [https://leetcode.com/problems/largest-perimeter-triangle/](https://leetcode.com/problems/largest-perimeter-triangle/)

## Examples
```
Example 1
Input: nums = [2,1,2]
Output: 5
```

```
Example 2
Input: nums = [1,2,1]
Output: 0
```

## Analysis
The first step is to sort the given array.
The examples show an array of just 3 elements, but as in the constraints, the array is much longer.
To find 3 sides, shift 3 values one by one.
This is because `a + b > c` is a condition.
If the current longest doesn't satisfy the condition, the longest should be switched to the next longest.
Without changing the longest c, changing a or b might work.
However, a + b + c must be the largest. So, shifting values one by one gives us the answer.

## Solution
```python
class LargestPerimeterTriangle:
    def largestPerimeter(self, nums: List[int]) -> int:
        nums.sort()
        for i in range(len(nums) - 3, -1, -1):
            if nums[i] + nums[i + 1] > nums[i + 2]:
                return nums[i] + nums[i + 1] + nums[i + 2]
        return 0
```

## Complexities
- Time: `O(nlog(n))`
- Space: `O(1)`

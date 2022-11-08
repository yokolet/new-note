---
layout: post
title: Trapping Rain Water
hero_height: is-small
tags:
- Hard
- Two Pointers
- Array
date: 2022-09-18 21:00 +0900
---
## Introduction
This problem might look like monotonic stack or dynamic programming.
Both work, however, two pointers approach also works well.
The trapped water is bounded by a lower height of left and right.
Keep tracking the lower height is a key to solve this problem

## Problem Description
> Given `n` non-negative integers representing an elevation map where the width of each bar is 1,
> compute how much water it can trap after raining.
>
> Constraints:
> - `n == height.length`
> - `1 <= n <= 2 * 10**4`
> - `0 <= height[i] <= 10**5`
>
> [https://leetcode.com/problems/trapping-rain-water/](https://leetcode.com/problems/trapping-rain-water/)

## Examples
```
Example 1
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: 6 units of rain water (blue section) are being trapped.
```

```
Example 2
Input: height = [4,2,0,3,2,5]
Output: 9
```

## Analysis

Here, two pointers approach is taken.
The pointers start from leftmost and rightmost indicies.
Set initial max heights for both left and right with the value of leftmost and rightmost indices.
The total trapped water is bounded by the lower height,
so take (the lower side max height - bar height at lower side height) and add it up to the total.
Then increment the pointer and maintain the max heights.

## Solution
```python
class TrappingRainWater:
    def trap(self, height: List[int]) -> int:
        n  = len(height)
        if n < 3:
            return 0
        left, right, total = 0, n - 1, 0
        left_max, right_max = height[left], height[right]
        while left < right:
            if left_max <= right_max:
                total += left_max - height[left]
                left += 1
                left_max = max(left_max, height[left])
            else:
                total += right_max - height[right]
                right -= 1
                right_max = max(right_max, height[right])
        return total
```

## Complexities
- Time: `O(n)`
- Space: `O(1)`
 

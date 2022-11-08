---
layout: post
title: Sliding Window Median
hero_height: is-small
tags:
- Hard
- Sliding Window
- Binary Search
- Array
date: 2022-10-14 22:47 +0900
---
## Introduction
For this problem, a few of approaches exist.
For example, two heaps, two pointers, or even sorting every subarray.
To think an efficient solution, we should think how to avoid duplicated operations.
The solution here took the binary search approach.
Since the subarray (window) should be sorted, the binary search and binary search based insertion works.

## Problem Description
> The median is the middle value in an ordered integer list. If the size of the list is even,
> there is no middle value. So the median is the mean of the two middle values.
> - For examples, if `arr = [2,3,4]`, the median is `3`.
> - For examples, if `arr = [1,2,3,4]`, the median is `(2 + 3) / 2 = 2.5`.
>
> You are given an integer array `nums` and an integer `k`. There is a sliding window of size `k` which is
> moving from the very left of the array to the very right. You can only see the `k` numbers in the window.
> Each time the sliding window moves right by one position.
>
> Return the median array for each window in the original array. Answers within 10**-5 of the actual value
> will be accepted.
>
> Constraints:
> - `1 <= k <= nums.length <= 10**5`
> - `-2**31 <= nums[i] <= 2**31 - 1`
>
> [https://leetcode.com/problems/sliding-window-median/](https://leetcode.com/problems/sliding-window-median/)

## Examples
```
Example 1
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [1.00000,-1.00000,-1.00000,3.00000,5.00000,6.00000]
Explanation: 
Window position                Median
---------------                -----
[1  3  -1] -3  5  3  6  7        1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7        3
 1  3  -1  -3 [5  3  6] 7        5
 1  3  -1  -3  5 [3  6  7]       6
```

```
Example 2
Input: nums = [1,2,3,4,2,3,1,4,2], k = 3
Output: [2.00000,3.00000,3.00000,3.00000,2.00000,3.00000,2.00000]
```

```
Example 3
Input: nums = [1,4,2,3], k = 4
Output: [2.50000]
```

## Analysis
Before starting the window shift, the solution here creates the window and find the first median value.
In the loop, it finds the index to be removed by bisect search and pops it from the window.
Next step is to insert a new value to the window using bisect insort.
Now, it's easy to find the median value of the current window.

## Solution
```python
class SlidingWidnowMedian:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        result = []
        window = sorted(nums[:k])
        result.append(window[k // 2] if k & 1 else (window[k // 2] + window[k //2 - 1]) / 2.0)
        for i in range(k, len(nums)):
            window.pop(bisect.bisect_left(window, nums[i - k]))
            bisect.insort(window, nums[i])
            result.append(window[k // 2] if k & 1 else (window[k // 2] + window[k //2 - 1]) / 2.0)
        return result
```

## Complexities
- Time: `O(nlog(n))`
- Space: `O(k)`

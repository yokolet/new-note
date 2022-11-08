---
layout: post
title: Minimum Swaps to Group All 1's Together
hero_height: is-small
tags:
- Medium
- Sliding Window
- Array
date: 2022-09-18 12:13 +0900
---
## Introduction
Some of array related problems ask minimum swaps to rearrange something.
If the test cases are easy ones, simulation might work.
However, the simulation is almost always not a good solution of this type.
If we think the final result after swaps,
counting in some range using two pointers might be much better solution.

## Problem Description
> Given a binary array `data`, return the minimum number of swaps
> required to group all 1’s present in the array together in any place in the array.
>
> Constraints:
> - `1 <= data.length <= 10**5`
> - `data[i]` is either 0 or 1.
>
> [https://leetcode.com/problems/minimum-swaps-to-group-all-1s-together/](https://leetcode.com/problems/minimum-swaps-to-group-all-1s-together/)

## Examples
```
Example 1
Input: data = [1,0,1,0,1]
Output: 1
Explanation: There are 3 ways to group all 1's together:
[1,1,1,0,0] using 1 swap.
[0,1,1,1,0] using 2 swaps.
[0,0,1,1,1] using 1 swap.
The minimum is 1.
```

```
Example 2
Input: data = [0,0,0,1,0]
Output: 0
Explanation: Since there is only one 1 in the array, no swaps are needed.
```

```
Example 3
Input: data = [1,0,1,0,1,0,0,1,1,0,1]
Output: 3
Explanation: One possible solution that uses 3 swaps is [0,0,0,0,0,1,1,1,1,1,1].
```

## Analysis
We should put all 1's in some area of the array.
It is not necessary at the end or beginning.
For example the given array is `[1, 0, 1, 0, 1]`, `[0, 1, 1, 1, 0]` is one of the final state.
From index 1 to 3, all 1's are put together.
In another word, a sum of index 1 to 3 is 3.
Given that, by looking at the range sum, we know how many swaps are needed in the range.

The first step is to get a sum of the given array, which is the same as the number of 1's.
Also, the sum is the same range all 1's put together.
Start the sum from index 0 to sum of the array.
Use two pointers, left and right, both are incremented by one each iteration.
Update the current sum, then compare maximum swaps.
Notice, it is the maximum not minimum.
In the end, the sum of the array value minus maximum swaps gives us the answer.

## Solution
```python
class MinimumSwapsToGroupAll1sTogether:
    def minSwaps(self, data: List[int]) -> int:
        n = len(data)
        ones = sum(data)
        cur = sum(data[0:ones])
        max_v = cur
        for right in range(ones, n):
            left = right - ones
            cur -= data[left]
            cur += data[right]
            max_v = max(max_v, cur)
        return ones - max_v
```

## Complexities
- Time: `O(n)`
- Space: `O(1)`
 

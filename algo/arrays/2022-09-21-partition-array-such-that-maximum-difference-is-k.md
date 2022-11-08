---
layout: post
title: Partition Array Such That Maximum Difference Is K
hero_height: is-small
tags:
- Medium
- Sorting
- Greedy
- Array
date: 2022-09-21 22:26 +0900
---
## Introduction
Since the given array's order does not matter, start sorting it.
We should focus only minimum and maximum in the range.
The idea of two pointers, left and right is useful here.
The minimum value (left) is fixed unless the difference between maximum (right) so far doesn't exceed the given threshold.

## Problem Description
> You are given an integer array `nums` and an integer `k`.
> You may partition `nums` into one or more subsequences such that
> each element in nums appears in exactly one of the subsequences.
>
> Return the minimum number of subsequences needed such that the difference between the maximum and minimum values
> in each subsequence is at most `k`.
>
> A subsequence is a sequence that can be derived from another sequence by deleting some or no elements
> without changing the order of the remaining elements.
>
> Constraints:
> - `1 <= nums.length <= 10**5`
> - `0 <= nums[i] <= 10**5`
> - `0 <= k <= 10**5`
>
> [https://leetcode.com/problems/partition-array-such-that-maximum-difference-is-k/](https://leetcode.com/problems/partition-array-such-that-maximum-difference-is-k/)

## Examples
```
Example 1
Input: nums = [3,6,1,2,5], k = 2
Output: 2
Explanation:
We can partition nums into the two subsequences [3,1,2] and [6,5].
```

```
Example 2
Input: nums = [1,2,3], k = 1
Output: 2
Explanation:
We can partition nums into the two subsequences [1,2] and [3].
```

```
Example 3
Input: nums = [2,2,4,5], k = 0
Output: 3
Explanation:
We can partition nums into the three subsequences [2,2], [4], and [5].
```

## Analysis
The first step is to sort the given array.
The initial minimum value is at index 0 since the array is sorted.
Going over sorted values one by one checking the difference between current and minimum values.
When it exceeds, update minimum values and count up.

## Solution
```python
class PartitionArraySuchThatMaximumDifferenceIsK:
    def partitionArray(self, nums: List[int], k: int) -> int:
        nums.sort()
        cur_min, count = nums[0], 1
        for num in nums:
            if num - cur_min <= k:
                continue
            cur_min = num
            count += 1
        return count
```

## Complexities
- Time: `O(nlog(n))`
- Space: `O(1)`

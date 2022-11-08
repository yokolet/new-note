---
layout: post
title: Subarray Sum Equals K
hero_height: is-small
tags:
- Medium
- Prefix Sum
- Hash Table
- Array
date: 2022-10-26 15:04 +0900
---
## Introduction
The problem gives us an array and asks about sum which is k.
This signals the prefix sum and hash table.
It is a kind of 2-sum problem.
That's why the hash table works.
The keys of the hash table are accumulated sums.
While going over the given array, if accumulated sum minus k exists in the table, the sum equals to k is found.

## Problem Description
> Given an array of integers `nums` and an integer `k`, return the total number of subarrays whose sum equals to `k`.
>
> A subarray is a contiguous non-empty sequence of elements within an array.
>
> Constraints:
> - `1 <= nums.length <= 2 * 10**4`
> - `-1000 <= nums[i] <= 1000`
> - `-10**7 <= k <= 10**7`
>
> [https://leetcode.com/problems/subarray-sum-equals-k/](https://leetcode.com/problems/subarray-sum-equals-k/)

## Examples
```
Example 1
Input: nums = [1,1,1], k = 2
Output: 2
```

```
Example 2
Input: nums = [1,2,3], k = 3
Output: 2
```

## Analysis
The solution here starts from initializing the accumulated value and hash table.
The value of the hash table is a number of times the accumulated value appeared.
So, the initial value is sum 0 and count 1.
While checking the array value one by one, find accumulated value minus k.
Like 2-sum problem, the accumulated sum minus k exists in the hash table, add up the count.
Then, count up at the current accumulated sum.

## Solution
```python
class SubarraySumEqualsK:
    def subarraySum(self, nums: List[int], k: int) -> int:
        acc, memo = 0, {0: 1}
        count = 0
        for v in nums:
            acc += v
            diff = acc - k
            if diff in memo:
                count += memo[diff]
            if acc in memo:
                memo[acc] += 1
            else:
                memo[acc] = 1
        return count
```

## Complexities
- Time: `O(n)`
- Space: `O(n)`

---
layout: post
title: Contains Duplicate II
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Hash Table
- Sliding Window
- Array
date: 2022-10-21 13:59 +0900
---
## Introduction
This is not a difficult problem.
However, making the solution efficient needs a bit of consideration.
It can be solved by multiple O(n) iterations, but can be done just one O(n) loop.
When we check all values from index 0 to n, it is ordered.
What we care about is the last index of the same value only.
When the same value appears, check the last index.
If the difference is less than or equals to k, return True.

## Problem Description
> Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` in the
> array such that `nums[i] == nums[j]` and `abs(i - j) <= k`.
>
> Constraints:
> - `1 <= nums.length <= 10**5`
> - `-10**9 <= nums[i] <= 10**9`
> - `0 <= k <= 10**5`
>
> [https://leetcode.com/problems/contains-duplicate-ii/](https://leetcode.com/problems/contains-duplicate-ii/)

## Examples
```
Example 1
Input: nums = [1,2,3,1], k = 3
Output: true
```

```
Example 2
Input: nums = [1,0,1,1], k = 1
Output: true
```

```
Example 3
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

## Analysis
The Hash Table is a good data structure to save the value and its last index pair.
When the same value appears, it is in the Hash Table.
So, check the difference between the last and current index.
If it is less than or equals to k, the answer is True.

## Solution
```python
class ContainsDuplicate2:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        counts = {}
        for idx, v in enumerate(nums):
            if v in counts and idx - counts[v] <= k:
                return True
            counts[v] = idx
        return False
```

## Complexities
- Time: `O(n)`
- Space: `O(n)`

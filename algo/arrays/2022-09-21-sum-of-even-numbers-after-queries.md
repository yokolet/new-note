---
layout: post
title: Sum of Even Numbers After Queries
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Array
- Simulation
date: 2022-09-21 14:16 +0900
---
## Introduction
This problem asks to save all calculation results after queries are performed.
This means a simulation is the way to solve the problem.
Considering the input array and queries might be very large, how to effectively calculate is a key.

## Problem Description
> You are given an integer array `nums` and an array `queries` where `queries[i] = [val[i], index[i]]`.
>
> For each query `i`, first, apply `nums[index[i]] = nums[index[i]] + val[i]`,
> then print the sum of the even values of nums.
>
> Return an integer array `answer` where `answer[i]` is the answer to the i-th query.
>
> Constraints:
> - `1 <= nums.length <= 10**4`
> - `-10**4 <= nums[i] <= 10**4`
> - `1 <= queries.length <= 10**4`
> - `-10**4 <= val[i] <= 10**4`
>
> [https://leetcode.com/problems/sum-of-even-numbers-after-queries/](https://leetcode.com/problems/sum-of-even-numbers-after-queries/)

## Examples
```
Example 1
Input: nums = [1,2,3,4], queries = [[1,0],[-3,1],[-4,0],[2,3]]
Output: [8,6,2,4]
Explanation: At the beginning, the array is [1,2,3,4].
query 0, nums: [2,2,3,4], even sum: 8
query 1, nums: [2,-1,3,4], even sum: 6
query 2, nums: [-2,-1,3,4], even sum: 2
query 3, nums: [-2,-1,3,6], even sum: 4
```

```
Example 2
Input: nums = [1], queries = [[4,0]]
Output: [0]
```

## Analysis
The first step is to get input array's even number sum.
After that, we should calculate only difference portion.
If the value at the query index is even, subtract it from the sum since updated value might be an odd.
If the value at the query index becomes even after operation, add it up to the sum.
Also, update the value at index and add the sum to answer list.
This way, we get get the answer.

## Solution
```python
class SumOfEvenNumbersAfterQueries:
    def sumEvenAfterQueries(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        even_sum, n = 0, len(nums)
        for i in range(n):
            if nums[i] % 2 == 0:
                even_sum += nums[i]
        answer = []
        for v, idx in queries:
            cur_v, new_v = nums[idx], nums[idx] + v
            if cur_v & 1 == 0:
                even_sum -= cur_v
            if new_v & 1 == 0:
                even_sum += new_v
            nums[idx] = new_v
            answer.append(even_sum)
        return answer
```

## Complexities
- Time: `O(n)`
- Space: `O(1)` -- no extra space added

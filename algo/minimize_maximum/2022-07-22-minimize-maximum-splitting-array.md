---
layout: post
title: Minimize Maximum -- Splitting Array
date: 2022-07-22 16:08 +0900
hero_height: is-small
tags: [Hard, Binary Search, Array, DP, Greedy]
---
## Introduction
Some algorithm questions ask this sort – minimize the maximum.
In general, problems give an array and a value or values which will be a clue to split the array. The key to solve this sort is to figure out what is the criteria to split. The patterns are similar, but we see many variations.

Well, how to solve this sort? At a glance, it looks like a dynamic programming problem.
Using memoization, bottom-up manner would lead to an answer. However, surprisingly, the binary search approach works very well.

Including this post, splitting an array, I’ll discuss about some of minimize maximum pattern problems.

## Problem Description
> Given an array nums which consists of non-negative integers and an integer m,
> you can split the array into m non-empty continuous subarrays.
> Write an algorithm to minimize the largest sum among these m subarrays.
>
> [https://leetcode.com/problems/split-array-largest-sum/](https://leetcode.com/problems/split-array-largest-sum/)

## Examples
```
example 1:
Input: nums = [7,2,5,10,8], m = 2
Answer: 18
Explanation: splitted array: [[7,2,5],[10,8]], sums: [14, 18]
18 is the minimized largest sum when the array is splitted into 2 groups
```
```
example 2:
Input: nums = [1,2,3,4,5], m = 2
Answer: 9
Explanation: splitted array: [[1,2,3],[4,5]], sums: [6, 9]
9 is the minimized largest sum when the array is splitted into 2 groups
```
```
example 3:
Input: nums = [1,4,4], m = 3
Answer: 4
Explanation: splitted array: [[1],[4],[4]], sums: [1,4,4]
4 is the minimized largest sum when the array is splitted into 3 groups
```

## Analysis

The given array should be divided into groups based on a certain criteria.
In this case, the criteria is the subarray sum.
It lies between the largest value in the array (left) and total sum of the array (right).
The iteration starts off from the middle of those left and right values.
Based on the middle value, the array will be split comparing the sum so far and the middle value.
In the end, some number of subarrays will be created.

Next step is to adjust the left or right values.
If the number of subarrays is less than the specified value, the middle value should be smaller to create more subarrays.
If the number of subarrays is greater than the specified value, the middle value should be greater to create less subarrays.
This way, we can find the answer.

## Solution
```cpp
#include <vector>
#include <iostream>
using namespace std;

class SplitArrayLargestSum
{
private:
  bool check(vector<int> &nums, int m, int mid)
  {
    int count = 1, cur = 0;
    for (int i = 0; i < nums.size(); ++i)
    {
      if (cur + nums[i] <= mid)
      {
        cur += nums[i];
      }
      else
      {
        ++count;
        cur = nums[i];
      }
    }
    return count <= m;
  }

public:
  int splitArray(vector<int> &nums, int m)
  {
    int n = nums.size();
    int sum = 0, max_elm = -1;
    for (int i = 0; i < nums.size(); ++i)
    {
      sum += nums[i];
      max_elm = max(max_elm, nums[i]);
    }
    int left = max_elm, right = sum;
    while (left <= right)
    {
      int mid = (left + right) / 2;
      if (check(nums, m, mid))
      {
        right = mid - 1;
      }
      else
      {
        left = mid + 1;
      }
    }
    return left;
  }
};
```

## Complexities
- Time: O(n log(s)) – s: sum of elements in the array
- Space: O(1)

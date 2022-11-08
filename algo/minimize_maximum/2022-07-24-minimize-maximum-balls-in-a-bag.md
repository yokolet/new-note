---
layout: post
title: Minimize Maximum -- Minimum Limit of Balls in a Bag
date: 2022-07-24 22:59 +0900
hero_height: is-small
tags: [Medium, Binary Search, Array]
---
## Introduction
This is the sixth minimize-maximum algorithm problem.
This problem has a variation compared to the basic, splitting array problem.
If we look around other minimize maximum problems, it is very close to the candies allocation to k children problem.
The problem might be as straightforward as the candies problem.
A difference is, the problem gives a number of operations rather than the number of groups to be created.
Again, the problem is the type of minimize-maximum.

## Problem Description
> You are given an integer array nums where the i-th bag contains `nums[i]` balls.
> You are also given an integer `maxOperations`.
>
> You can take any bag of balls and divide it into two new bags with a positive number of balls.
> Above operation can be performed at most maxOperations times.
> Example: `5` -> `[1, 4]` or `[2, 3]` by one operation.
> 
> Your penalty is the maximum number of balls in a bag.
> You want to minimize your penalty after the operations.
>
> Return the minimum possible penalty after performing the operations.
> 
> [https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/)

## Examples
```
example 1:
Input: nums = [9], maxOperations = 2
Answer: 3
Explanation: nums array goes: [9] -> [6,3] -> [3,3,3] by 2 operations. 
3 is the minimized largest penalty when the array elements are splitted.
```
```
example 2:
Input: nums = [2,4,8,2], maxOperations = 4
Answer: 2
Explanation: nums array goes:
[2,4,8,2] -> [2,4,4,4,2] -> [2,2,2,4,4,2] -> [2,2,2,2,2,4,2] -> [2,2,2,2,2,2,2,2] by 4 operations.
2 is the minimized largest penalty when the array elements are splitted.
```
```
example 3:
Input: nums = [7,17], maxOperations = 2
Answer: 7
Explanation: nums array goes: [7,17] -> [7,7,10] -> [7,7,3,7] by 2 operations.
7 is the minimized largest penalty when the array elements are splitted.
```

## Analysis
Each element of the given nums array should be divided into smaller number of pair based on a certain criteria.
The criteria is middle value of the array.

So, we will count how many elements can be divided in to two based on the middle value.
The smallest value (left) can be 1, and the largest (right) will be the maximum value in the given array.
The middle value is set to `(left + right) / 2` as normal.

Next step is to adjust the left or right values.
If the number of operations is less than the specified value, the middle value should be smaller to do more operations.
If the number of operations is greater than the specified value, the middle value should be greater to do less operations.
The left and right value updates follow a common way:  `right = mid - 1`, `left = mid + 1`;

When the binary search is over, we can find the answer.
The answer from the binary search is the left value.


## Solution
```cpp
#include <vector>
#include <numeric>
#include <iostream>
using namespace std;

class MinimumLimitOfBallsInABag
{
private:
  bool check(vector<int> &nums, int maxOperations, int mid)
  {
    int count = 0;
    for (int i = 0; i < nums.size(); ++i)
      {
        if (nums[i] >= mid)
        {
          count += ceil((double)nums[i] / mid) - 1;
        }
      }
      return count <= maxOperations;
  }
public:
  int minimumSize(vector<int> &nums, int maxOperations)
  {
    int n = nums.size();
    int left = 1, right = *max_element(nums.begin(), nums.end());
    while (left <= right)
    {
      int mid = (left + right) / 2;
      if (check(nums, maxOperations, mid))
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

---
layout: post
title: Minimize Maximum -- Maximum Candies Allocated to K Children
date: 2022-07-24 21:57 +0900
algo_menubar: algo_menu
hero_height: is-small
tags: [Medium, Binary Search, Array]
---
## Introduction
This is the fifth minimize-maximum algorithm problem.
This problem has a variation compared to the basic, splitting array problem.
If we look around other minimize maximum problems, it is very close to the cutting ribbons problem.
The problem might be more straightforward compared to the cutting ribbons.
Again, the problem is the type of minimize-maximum.

## Problem Description
> You are given a 0-indexed integer array candies.
> Each element in the array denotes a pile of candies of size candies[i].
> You can divide each pile into any number of sub piles, but you cannot merge two piles together.
>
> You are also given an integer k.
> You should allocate piles of candies to k children such that each child gets the same number of candies.
> Each child can take at most one pile of candies and some piles of candies may go unused.
>
> Return the maximum number of candies each child can get.
> 
> [https://leetcode.com/problems/maximum-candies-allocated-to-k-children/](https://leetcode.com/problems/maximum-candies-allocated-to-k-children/)

## Examples
```
example 1:
Input: candies = [5,8,6], k = 3
Answer: 5
Explanation: candies subarrays after splitting: [[5],[5,3],[5,1]].
5 is the minimized largest candies when the array is splitted.
```
```
example 2:
Input: candies = [2,5], k = 11
Answer: 0
Explanation: sum of all candies are 7. Even the shortest count of 1 won't make 11 groups.
```

## Analysis
Each element of the given candies array should be divided into smaller piles based on a certain criteria.
The criteria is a number of candies in each piles.

So, we will count how many candy piles are created based on the number of candies in each pile (middle value).
The smallest value (left) can be 1, and the largest (right) will be the maximum value in the given array.
The middle value is set to `(left + right) / 2` as normal.

Next step is to adjust the left or right values.
If the number of piles is less than the specified value, the middle value should be smaller to create more piles.
If the number of piles is greater than the specified value, the middle value should be greater to create less piles.
The left and right value updatea follow a common way:  `right = mid - 1`, `left = mid + 1`;

When the binary search is over, we can find the answer.
Here, we should be careful. The answer from the binary search is the right value.

## Solution
```cpp
#include <vector>
#include <iostream>
using namespace std;

class MaximumCandiesAllocatedToKChildren
{
private:
  bool check(vector<int> &candies, long long k, long long mid)
  {
    long long count = 0;
    for (int i = 0; i < candies.size(); ++i)
    {
      count += candies[i] / mid;
    }
    return count < k;
  }

public:
  int maximumCandies(vector<int> &candies, long long k)
  {
    int n = candies.size();
    int left = 1, right = *max_element(candies.begin(), candies.end());
    while (left <= right)
    {
      long long mid = (left + right) / 2;
      if (check(candies, k, mid))
      {
        right = mid - 1;
      }
      else
      {
        left = mid + 1;
      }
    }
    return right;
  }
};
```

## Complexities
- Time: O(n log(s)) – s: total number of divided piles
- Space: O(1)

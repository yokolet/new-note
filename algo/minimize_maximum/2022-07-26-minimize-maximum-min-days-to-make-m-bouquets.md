---
layout: post
title: Minimize Maximum -- Minimum Number of Days to Make m Bouquets
date: 2022-07-26 13:52 +0900
hero_height: is-small
tags: [Medium, Binary Search, Array]
---
## Introduction
This is the eighth minimize-maximum algorithm problem.
This problem has a variation compared to the basic, splitting array problem.
However, still, it is similar to the splitting array problem if we look at
how to count groups.
Again, the problem is the type of minimize-maximum.

## Problem Description
> You are given an integer array `bloomDay`, an integer `m` and an integer `k`.
>
> You want to make m bouquets.
> To make a bouquet, you need to use k adjacent flowers from the garden.
>
> The garden consists of n flowers, the i-th flower will bloom in the `bloomDay[i]` and then can be used in exactly one bouquet.
>
> Return the minimum number of days you need to wait to be able to make `m` bouquets from the garden.
> If it is impossible to make m bouquets return -1.
>
> [https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)

## Examples
```
example 1:
Input: bloomDay = [1,10,3,10,2], m = 3, k = 1
Answer: 3
Explanation: The optimal minimum days are 3. After day 3, usable flowers are [f,_,f,_,f].
Those are enough to create 3 bouquets one type each.
```
```
example 2:
Input: bloomDay = [1,10,3,10,2], m = 3, k = 2
Answer: -1
Explanation: To make 3 bouquets 2 flowers each, which requires 3 * 2 = 6 flowers.
From the problem description, the flowers of a single index can be usesd in exactly one bouquet.
The given arrays has only 5 types, so it's unable to use 6 flowers.
```
```
example 3:
Input: bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3
Answer: 12
Explanation: The optimal minimum days are 12. After day 7, usable flowers are [f,f,f,f,_,f,f].
Two bouquets can be created with 4 and 2 types, which doesn't satisfy the condition of 3 types each.
After day 12, usable fowers are [f,f,f,f,f,f,f], which is enough to create 3 bouquets of 3 types.
```

## Analysis
Each element of the given array will be used to count groups to be created.
The element value itself isn't involved in the counting.
Instead, it works as a filter and separates groups depends on the day.

When we count the bouquets, first, count the number of consecutive elements.
Then, dividing the count by k gives us the number of bouquets so far.

When we start the binary search, parameters are as in bellow.
The smallest value (left) is 1, and the largest (right) is the maximum value in the given array.
The middle value is set to `(left + right) / 2` as normal.

Next step is to adjust the left or right values.
The middle value is used as a filter.
If the element in the array is less than or equals to the middle value, that element forms a subarray.
The length of the subarray divided by k is the number of bouquets from this subarray.
If total number of bouquets is greater than `m`, 
the middle value should be smaller to include less elements and create less bouquets.
If total number of bouquets is less than `m`,
the middle value should be bigger to include more element and create more bouquets.
The left and right value updates follow a common way:  `right = mid - 1`, `left = mid + 1`;

When the binary search is over, we can find the answer.
The answer from the binary search is the left value.

## Solution
```cpp
#include <vector>
#include <iostream>
using namespace std;

class MinimumNumberOfDaysToMakeMBouquets
{
private:
  bool check(vector<int> &bloomDay, int m, int k, int mid)
  {
    int count = 0, cur = 0;
    for (int i = 0; i < bloomDay.size(); ++i)
    {
      if (bloomDay[i] <= mid)
      {
        ++cur;
      }
      else
      {
        count += cur / k;
        cur = 0;
      }
    }
    count += cur / k;
    return count >= m;
  }

public:
  int minDays(vector<int> &bloomDay, int m, int k)
  {
    int n = bloomDay.size();
    if (m * k > n)
      return -1;
    int left = 1, right = *max_element(bloomDay.begin(), bloomDay.end());
    while (left <= right)
    {
      int mid = (left + right) / 2;
      if (check(bloomDay, m, k, mid))
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
- Time: O(n log(m)) – n: length of the input array, m: maximum value in the input array
- Space: O(1)

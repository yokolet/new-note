---
layout: post
title: Minimize Maximum -- Koko Eating Bananas
date: 2022-07-25 23:26 +0900
algo_menubar: algo_menu
hero_height: is-small
tags: [Medium, Binary Search, Array]
---
## Introduction
This is the seventh minimize-maximum algorithm problem.
This problem has a variation compared to the basic, splitting array problem.
If we look around other minimize maximum problems, it is very close to the balls in a bag problem.
The problem looks almost the same idea as the balls in a bag problem.
A difference is, the problem doesn't split elements in the given array.
Again, the problem is the type of minimize-maximum.

## Problem Description
> Koko loves to eat bananas. There are `n` piles of bananas, the i-th pile has `piles[i]` bananas.
> The guards have gone and will come back in `h` hours.
>
> Koko can decide her bananas-per-hour eating speed of `k`.
> Each hour, she chooses some pile of bananas and eats `k` bananas from that pile.
> If the pile has less than k bananas, she eats all of them instead and will not eat any more bananas during this hour.
>
> Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.
>
> Return the minimum integer k such that she can eat all the bananas within `h` hours.
>
> [https://leetcode.com/problems/koko-eating-bananas/](https://leetcode.com/problems/koko-eating-bananas/)

## Examples
```
example 1:
Input: piles = [3,6,7,11], h = 8
Answer: 4
Explanation: The optimal minimum eating speed is 4. At this speed, it will take 3/4->1, 6/4->2, 7/4->2, 11/4->3 hours each.
Summing up the each hours, 1 + 2 + 2 + 3 = 8. All bananas can be eaten before a guard comes back in 8 hours.
In another view, it creates subarrays: [[3],[4,2],[4,3],[4,4,3]]. The lengths of subarrays are [1,2,2,3].
Summing up the subarray lengths, we will get 8 which is the hour guard returns.
```
```
example 2:
Input: piles = [30,11,23,4,20], h = 5
Answer: 30
Explanation: Explanation: The optimal minimum eating speed is 30. At this speed, it will take
30/30->1, 11/30->1, 23/30->1, 4/30->1, 20/30->1 hours each.
Summing up the each hours, 1 + 1 + 1 + 1 + 1 = 5. All bananas can be eaten before a guard comes back in 5 hours.
```
```
example 3:
Input: piles = [30,11,23,4,20], h = 6
Answer: 23
Explanation: The optimal minimum eating speed is 23. At this speed, it will take
30/23->2, 11/23->1, 23/23->1, 4/23->1, 20/23->1 hours each.
Summing up the each hours, 2 + 1 + 1 + 1 + 1 = 6. All bananas can be eaten before a guard comes back in 6 hours.
```

## Analysis
Each element of the given piles array will be used to calculate eating speed based on a certain criteria.
The criteria is middle value of the array.

So, we will count necessary hours to consume each elements when consume speed is the middle value.
If a division leaves a reminder, it needs plus one hour for the pile.
The smallest value (left) can be 1, and the largest (right) will be the maximum value in the given array.
The middle value is set to `(left + right) / 2` as normal.

Next step is to adjust the left or right values.
If the total hours to consume bananas is less than the specified value,
the middle value should be smaller to use more total hours.
If the total hours to consume bananas is greater than the specified value,
the middle value should be greater to use less hours.
The left and right value updates follow a common way:  `right = mid - 1`, `left = mid + 1`;

When the binary search is over, we can find the answer.
The answer from the binary search is the left value.

## Solution
```cpp
#include <vector>
#include <numeric>
#include <iostream>
using namespace std;

class KokoEatingBananas
{
private:
  bool check(vector<int> &piles, int h, int mid)
  {
    int count = 0;
    for (int i = 0; i < piles.size(); ++i)
    {
      count += ceil((double)piles[i] / mid);
    }
    return count <= h;
  }

public:
  int minEatingSpeed(vector<int> &piles, int h)
  {
    int n = piles.size();
    int left = 1, right = *max_element(piles.begin(), piles.end());
    while (left <= right)
    {
      int mid = (left + right) / 2;
      if (check(piles, h, mid))
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

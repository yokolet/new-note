---
layout: post
title: Best Time to Buy and Sell Stock -- Basic
date: 2022-07-26 15:50 +0900
hero_height: is-small
tags: [Easy, Array, DP]
---
## Introduction
The algorithm questions such that best time to buy and sell stock has variations.
Each has slight different criteria to find the answer.
This is the first buy-sell-stock problem and very basic easy one.

## Problem Description
> You are given an array `prices` where `prices[i]` is the price of a given stock on the i-th day.
>
> You want to maximize your profit by choosing a single day to buy one stock and choosing a different day
> in the future to sell that stock.
>
> Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.
> 
> [https://leetcode.com/problems/best-time-to-buy-and-sell-stock/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

## Examples
```
example 1:
Input: prices = [7,1,5,3,6,4]
Answer: 5
Explanation: 5 is the best profit which is from buying on the index 1 an selling on the index 4.
```
```
example 2:
Input: prices = [7,6,4,3,1]
Answer: 0
Explanation: the array value is ever descreasing. There is no profit available.
```

## Analysis
We are allowed to buy and sell only once. The minimum price so far should be updated in each array index.
If the price at the current index is not the minimum, calculate profit so far.
Then update grand max profit comparing with the current profit.

## Solution
```cpp
#include <vector>
#include <iostream>
using namespace std;

class BestTimeToBuyAndSellStock
{
public:
  int maxProfit(vector<int> &prices)
  {
    int min_v = INT_MAX;
    int max_profit = 0;
    for (int i = 0; i < prices.size(); ++i)
    {
      if (prices[i] < min_v)
      {
        min_v = prices[i];
      }
      else
      {
        max_profit = max(max_profit, prices[i] - min_v);
      }
    }
    return max_profit;
  }
};
```

## Complexities
- Time: O(n) – n: length of the input array.
- Space: O(1)

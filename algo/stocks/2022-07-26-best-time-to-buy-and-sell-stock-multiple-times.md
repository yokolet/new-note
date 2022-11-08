---
layout: post
title: Best Time to Buy and Sell Stock -- Multiple Times
date: 2022-07-26 16:10 +0900
hero_height: is-small
tags: [Medium, Array, DP, Greedy]
---
## Introduction
This is the second buy-sell-stock problem which has a slight variation to the basic one.
In this problem, we can buy and sell stock as many times we want.

## Problem Description
> You are given an integer array `prices` where `prices[i]` is the price of a given stock on the i-th day.
>
> On each day, you may decide to buy and/or sell the stock.
> You can only hold at most one share of the stock at any time.
> However, you can buy it then immediately sell it on the same day.
>
> Find and return the maximum profit you can achieve.
> 
> [https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

## Examples
```
example 1:
Input: prices = [7,1,5,3,6,4]
Answer: 7
Explanation: 7 is the best profit which is from:
buy: index 1, sell: index 2, profit: 4
buy: index 3, sell: index 4, profit: 3
```
```
example 2:
Input: prices = [1,2,3,4,5]
Answer: 4
Explanation: 4 is the best profit which is from buying on the index 0 an selling on the index 4.
```
```
example 3:
Input: prices = [7,6,4,3,1]
Answer: 0
Explanation: the array value is ever descreasing. There is no profit available.
```

## Analysis
This is an interesting problem. It looks we should find multiple minimums and compare profits.
It might easily go to O(n^2), brute force solution.
However, if we look at the difference of each day, we should add up every day's profit unless it it not negative.
This can be done because we can sell and buy at the same day.
It turns out, the solution is simple.

## Solution
```cpp
#include <vector>
#include <iostream>
using namespace std;

class BestTimeToBuyAndSellStockTwo
{
public:
  int maxProfit(vector<int> &prices)
  {
    if (prices.empty())
      return 0;
    int max_v = 0;
    for (int i = 1; i <prices.size(); ++i)
    {
      if (prices[i] > prices[i - 1])
      {
        max_v += prices[i] - prices[i - 1];
      }
    }
    return max_v;
  }
};
```

## Complexities
- Time: O(n) – n: length of the input array.
- Space: O(1)

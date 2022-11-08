---
layout: post
title: Best Time to Buy and Sell Stock -- Two Transactions
date: 2022-07-26 19:58 +0900
hero_height: is-small
tags: [Hard, Array]
---
## Introduction
This is the third buy-sell-stock problem which has a slight variation to the basic one.
In this problem, we can buy and sell stock at most twice.
This is not an easy problem.
However, if we focus on value differences in the array, we can find a couple of approaches to solve this.

## Problem Description
> You are given an array `prices` where `prices[i]` is the price of a given stock on the i-th day.
>
> Find the maximum profit you can achieve. You may complete at most two transactions.
> 
> [https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## Examples
```
example 1:
Input: prices = [3,3,5,0,0,3,1,4]
Answer: 6
Explanation: 6 is the best profit which is from:
buy: index 3, sell: index 5, profit: 3
buy: index 6, sell: index 7, profit: 3
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
Among some approaches, going over left to right, right to left and combine those might be an easy one.
Like the basic buy and sell stock problem, calculate max profit and save it for memoization.
Then, going over from right to left to calculate max profit ans save it in another vector for memoization.
When the max profit is calculated from right side, it sees how much decreased.
Lastly, sum of left max profits and one index shifted right max profits are compared.
The right profits came from one index shifted to the right, which means ranges are not overwrapped.
That's how max profit from at most two transactions will be found.

## Solution
```cpp
class BestTimeToBuyAndSellStockThree
{
public:
  int maxProfit(vector<int> &prices)
  {
    int n = prices.size();
    vector<int> left(n, 0), right(n + 1, 0);
    int left_min = prices[0], right_max = prices[n - 1];
    for (int i = 1; i < n; ++i)
    {
      left[i] = max(left[i - 1], prices[i] - left_min);
      left_min = min(left_min, prices[i]);
    }
    for (int i = n - 2; i >= 0; --i)
    {
      right[i] = max(right[i + 1], right_max - prices[i]);
      right_max = max(right_max, prices[i]);
    }
    int max_profit = 0;
    for (int i = 0; i < n; ++i)
    {
      max_profit = max(max_profit, left[i] + right[i + 1]);
    }
    return max_profit;
  }
};
```

## Complexities
- Time: O(n) – n: length of the input array.
- Space: O(n)

---
layout: post
title: Best Time to Buy and Sell Stock -- with Cooldown
date: 2022-07-27 21:29 +0900
hero_height: is-small
tags: [Medium, Array, DP]
---
## Introduction
This is the fifth buy-sell-stock problem which has a slight variation to the basic one.
In this problem, we need to hold a day after the stocks are sold.
Aside of that, we can buy and sell as many times as we want.
That being said, the problem looks similar to the buy and sell multiple times.

## Problem Description
> You are given an array `prices` where `prices[i]` is the price of a given stock on the i-th day.
>
> Find the maximum profit you can achieve.
> You may complete as many transactions as you like
> (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:
>
> > After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
> 
> Note: You may not engage in multiple transactions simultaneously
> (i.e., you must sell the stock before you buy again).
> 
> [https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Examples
```
example 1:
Input: prices = [1,2,3,0,2]
Answer: 3
Explanation: 3 is the best profit which is from:
buy: index 0, sell: index 1, profit: 1
buy: index 3, sell: index 4, profit: 2
```
```
example 2:
Input: prices = [1]
Answer: 0
Explanation: Stocks must be bought before selling those. There's no way to make profit.
```

## Analysis
The the problem states, we can buy and sell as many time as we want.
This indicates the profit can be calculated by day-to-day difference.
However, we should put the cooldown day before buying next.
The solution uses the idea of DP: compare buying today or keep previous buy.
Selling stocks is the same as buying. It compares selling today or keep previous sell.

## Solution
```cpp
#include <vector>
#include <iostream>
using namespace std;

class BestTimeToBuyAndSellStockWithCooldown
{
public:
  int maxProfit(vector<int> &prices)
  {
    vector<int> buy(prices.size()), sell(prices.size()), rest(prices.size());
    buy[0] = -prices[0];
    for (int i = 1; i < prices.size(); i++)
    {
      // Held a previous stock OR buy this stock
      buy[i] = max(buy[i - 1], sell[i - 1] - prices[i]);
      // Reset, don't have any stock in hand, available to buy
      sell[i] = max(sell[i - 1], rest[i - 1]);
      // Sold, sell the stock
      rest[i] = buy[i - 1] + prices[i];
    }
    return max(sell[prices.size() - 1], rest[prices.size() - 1]);
  }
};
```

## Complexities
- Time: O(n) – n: length of the input array.
- Space: O(n)

---
layout: post
title: Best Time to Buy and Sell Stock -- Transaction Fee
date: 2022-07-27 22:34 +0900
algo_menubar: algo_menu
hero_height: is-small
tags: [Medium, Array, DP, Greedy]
---
## Introduction
This is the sixth buy-sell-stock problem which has a slight variation to the basic one.
In this problem, we need to pay a transaction fee everytime everytime we buy ans sell stocks.
Aside of that, we can buy and sell as many times as we want.
That being said, the problem looks similar to the buy and sell multiple times.

## Problem Description
> You are given an array `prices` where `prices[i]` is the price of a given stock on the i-th day,
> and an integer fee representing a transaction fee.
>
> Find the maximum profit you can achieve.
> You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.
>
> > Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
> 
> [https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)


## Examples
```
example 1:
Input: prices = [1,3,2,8,4,9], fee = 2
Answer: 8
Explanation: 8 is the best profit which is from:
buy: index 0, sell: index 3, profit: 8 - 1 - 2 = 5
buy: index 4, sell: index 5, profit: 9 - 4 - 2 = 3
```
```
example 2:
Input: prices = [1,3,7,5,10,3], fee = 3
Answer: 6
Explanation: 6 is the best profit which is from:
buy: index 0, sell: index 4, profit: 10 - 1 - 3 = 6
```

## Analysis
The the problem states, we can buy and sell as many time as we want.
This indicates the profit can be calculated by day-to-day difference.
However, we should pay the transaction fee to sell stocks.
The solution uses the idea of DP: compare previous sell and current buying cost.
Also, it compares previous buy and current selling cost.
The approach is very similar to the buy-sell-stock with cooldown problem.

## Solution
```cpp
#include <vector>
#include <iostream>
using namespace std;

class BestTimeToBuyAndSellStockWithTransactionFee
{
public:
  int maxProfit(vector<int> &prices, int fee)
  {
    int n = prices.size();
    vector<int> buy(n, 0), sell(n, 0);
    buy[0] = -prices[0];
    sell[0] = 0;
    for (int i = 1; i < prices.size(); ++i)
    {
      sell[i] = max(sell[i - 1], buy[i - 1] + prices[i] - fee);
      buy[i] = max(buy[i - 1], sell[i - 1] - prices[i]);
    }
    return sell[n - 1];
  }
};
```

## Complexities
- Time: O(n) – n: length of the input array.
- Space: O(n)

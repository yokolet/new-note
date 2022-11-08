---
layout: post
title: Stock Price Fluctuation
hero_height: is-small
tags:
- Medium
- Heap
- Hash Table
- Design
- Data Stream
- Ordered Set
date: 2022-09-07 15:42 +0900
---
## Introduction
This problem is a different type of stocks.
Stock prices are coming in one by one from data stream.
It means this is an online algorithm type.
Additionally, this is a design problem.
A class template is given with empty constructor and methods.

## Problem Description
> You are given a stream of `record`s about a particular stock.
> Each record contains a `timestamp` and the corresponding `price` of the stock at that timestamp.\
> Unfortunately due to the volatile nature of the stock market, the records do not come in order.
> Even worse, some records may be incorrect.
> Another record with the same timestamp may appear later in the stream correcting the price of
> the previous wrong record.
>
> Design an algorithm that:
>
> - Updates the price of the stock at a particular timestamp,
>   correcting the price from any previous records at the timestamp.
> - Finds the latest price of the stock based on the current records.
>   The latest price is the price at the latest timestamp recorded.
> - Finds the maximum price the stock has been based on the current records.
> - Finds the minimum price the stock has been based on the current records.
>
> Implement the StockPrice class:
>
> - `StockPrice()` Initializes the object with no price records.
> - `void update(int timestamp, int price)` Updates the price of the stock at the given timestamp.
> - `int current()` Returns the latest price of the stock.
> - `int maximum()` Returns the maximum price of the stock.
> - `int minimum()` Returns the minimum price of the stock.
> 
> Constraints:
> 
> `1 <= timestamp, price <= 10**9`\
> At most `10**5` calls will be made in total to `update`, `current`, `maximum`, and `minimum`.
> `current`, `maximum`, and `minimum` will be called only after `update` has been called at least once.
>
> [https://leetcode.com/problems/stock-price-fluctuation/](https://leetcode.com/problems/stock-price-fluctuation/)

## Examples
```
Example 1:
Input:
["StockPrice", "update", "update", "current", "maximum", "update", "maximum", "update", "minimum"]
[[], [1, 10], [2, 5], [], [], [1, 3], [], [4, 2], []]
Output:
[null, null, null, 5, 10, null, 5, null, 2]

Explanation:
StockPrice stockPrice = new StockPrice();
stockPrice.update(1, 10); // Timestamps are [1] with corresponding prices [10].
stockPrice.update(2, 5);  // Timestamps are [1,2] with corresponding prices [10,5].
stockPrice.current();     // return 5, the latest timestamp is 2 with the price being 5.
stockPrice.maximum();     // return 10, the maximum price is 10 at timestamp 1.
stockPrice.update(1, 3);  // The previous timestamp 1 had the wrong price, so it is updated to 3.
                          // Timestamps are [1,2] with corresponding prices [3,5].
stockPrice.maximum();     // return 5, the maximum price is 5 after the correction.
stockPrice.update(4, 2);  // Timestamps are [1,2,4] with corresponding prices [3,5,2].
stockPrice.minimum();     // return 2, the minimum price is 2 at timestamp 4.
```

## Analysis
If the problem is the type of online algorithm and asks maximum and/or minimum,
the heap data structure should be used.
The problem asks both maximum and minimum, we need min and max heaps.
To support current value reference, we should keep values in a hash map.

Each method should focus on only necessary operations.
Heap updates will be done when maximum or minimum methods are called.
In max/min heaps, old data are saved.
Those will be eliminated when heap pop result mismatches values in hash map.

## Solution
```python
class StockPrice:

    def __init__(self):
        self.curId = -1
        self.d = {}  # id: price
        self.maxheap = [] # (price, id)
        self.minheap = [] # (price, id)


    def update(self, timestamp: int, price: int) -> None:
        self.curId = max(self.curId, timestamp)
        self.d[timestamp] = price
        heapq.heappush(self.maxheap, (-price, timestamp))
        heapq.heappush(self.minheap, (price, timestamp))


    def current(self) -> int:
        return self.d[self.curId]


    def maximum(self) -> int:
        p, id = self.maxheap[0]
        while self.maxheap and self.d[id] != -p:
            heapq.heappop(self.maxheap)
            p, id = self.maxheap[0]
        return -p


    def minimum(self) -> int:
        p, id = self.minheap[0]
        while self.minheap and self.d[id] != p:
            heapq.heappop(self.minheap)
            p, id = self.minheap[0]
        return p
```

## Complexities
- Time: `O(nlog(n))`
- Space: `O(n)`
- 
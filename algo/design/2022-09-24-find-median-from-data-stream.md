---
layout: post
title: Find Median from Data Stream
hero_height: is-small
tags:
- Hard
- Design
- Heap (Priority Queue)
- Data Stream
- Sorting
- Two Pointers
date: 2022-09-24 20:15 +0900
---
## Introduction
A stream of data is the input for this problem.
In such a case, how to maintain the input data so far is a key to solve the problem.
The approach take here is two heaps which maintain smaller and bigger values compared to the median.
When a query comes, the heap data structure is able to return the answer by O(1).

## Problem Description
> The median is the middle value in an ordered integer list.
> If the size of the list is even, there is no middle value and the median is the mean of the two middle values.
> 
> - For example, for `arr = [2,3,4]`, the median is `3`.
> - For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.
>
> Implement the MedianFinder class:
> - `MedianFinder()` initializes the `MedianFinder` object.
> - `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
> - `double findMedian()` returns the median of all elements so far.
>    Answers within `10**(-5)` of the actual answer will be accepted.
>
> Constraints:
> - `-10**5 <= num <= 10**5`
> - There will be at least one element in the data structure before calling `findMedian`.
> - At most `5 * 10**4` calls will be made to `addNum` and `findMedian`.
>
> [https://leetcode.com/problems/find-median-from-data-stream/](https://leetcode.com/problems/find-median-from-data-stream/)

## Examples
```
Example 1

Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

## Analysis
The class has two heaps: max heap to maintain smaller values than median,
min heap to maintain bigger values then median.
The naming of heaps comes from Python's heap nature.
Python's heap is always non-decreasing order and no way around.
A common practice is save (-1)*value to make it non-increasing order.

We want to make min heap (bigger number) to larger or equal to max heap (smaller number).
If two heaps have the same size, min heap should have a new value.
At first, push -num to max heap, then pop from max heap. Finally, push (-1)*value to min heap.
If not (min heap is larger), max heap should have a new value.
At first, push num to min heap, then pop from min heap. Finally, push (-1)*value to max heap.

To answer the query, check how main data came in so far.
If it is odd, min heap top has the median.
If it is even, the average of min and max heap's top values is the median.

## Solution
```python
class MedianFinder:

    def __init__(self):
        self.count = 0
        self.maxheap = []
        self.minheap = []

    def addNum(self, num: int) -> None:
        if len(self.maxheap) == len(self.minheap):
            heapq.heappush(self.minheap,
                          -heapq.heappushpop(self.maxheap, -num))
        else:
            heapq.heappush(self.maxheap,
                          -heapq.heappushpop(self.minheap, num))
        self.count += 1

    def findMedian(self) -> float:
        if self.count % 2 == 1:
            return float(self.minheap[0])
        else:
            return (-self.maxheap[0] + self.minheap[0]) / 2.0
```

## Complexities
- Time: addNum: `O(log(n))`, findMedian: `O(1)`
- Space: `O(n)`

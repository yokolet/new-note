---
layout: post
title: Design Hit Counter
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Queue
- Design
date: 2022-09-26 22:08 +0900
---
## Introduction
The key to solve this problem is how to maintain hits' timestamp.
Since the timestamp is monotonically increasing -- sorted already,
the binary search to find an answer looks a good approach.
However, it can be much simple -- keep only hits from 300 seconds before.

## Problem Description
> Design a hit counter which counts the number of hits received in the past 5 minutes (i.e., the past 300 seconds).
>
> Your system should accept a `timestamp` parameter (in seconds granularity), and you may assume that
> calls are being made to the system in chronological order (i.e., timestamp is monotonically increasing).
> Several hits may arrive roughly at the same time.
>
> Implement the `HitCounter` class:
> - `HitCounter()` Initializes the object of the hit counter system.
> - `void hit(int timestamp)` Records a hit that happened at timestamp (in seconds). Several hits may happen at the same timestamp.
> - `int getHits(int timestamp)` Returns the number of hits in the past 5 minutes from timestamp (i.e., the past 300 seconds).

>
> Constraints:
> - `1 <= timestamp <= 2 * 10**9`
> - All the calls are being made to the system in chronological order (i.e., `timestamp` is monotonically increasing).
> - At most 300 calls will be made to `hit` and `getHits`.

> [https://leetcode.com/problems/design-hit-counter/](https://leetcode.com/problems/design-hit-counter/)

## Examples
```
Example 1
Input
["HitCounter", "hit", "hit", "hit", "getHits", "hit", "getHits", "getHits"]
[[], [1], [2], [3], [4], [300], [300], [301]]
Output
[null, null, null, null, 3, null, 4, 3]

Explanation
HitCounter hitCounter = new HitCounter();
hitCounter.hit(1);       // hit at timestamp 1.
hitCounter.hit(2);       // hit at timestamp 2.
hitCounter.hit(3);       // hit at timestamp 3.
hitCounter.getHits(4);   // get hits at timestamp 4, return 3.
hitCounter.hit(300);     // hit at timestamp 300.
hitCounter.getHits(300); // get hits at timestamp 300, return 4.
hitCounter.getHits(301); // get hits at timestamp 301, return 3.
```

## Analysis
The solution here creates a queue for timestamps.
Both, `hit` and `getHits` maintains the values in queue.
If the values are less than or equals to given timestamp - 300,
it deletes all hits before timestamp - 300.
The method, `gitHits`, returns the length of the queue. That's all.

## Solution
```python
class HitCounter:

    def __init__(self):
        self.queue = []

    def hit(self, timestamp: int) -> None:
        self.queue.append(timestamp)
        while self.queue[0] <= timestamp - 300:
            self.queue.pop(0)

    def getHits(self, timestamp: int) -> int:
        while self.queue and self.queue[0] <= timestamp - 300:
            self.queue.pop(0)
        return len(self.queue)
```

## Complexities
- Time: `O(1)`
- Space: `O(n)`

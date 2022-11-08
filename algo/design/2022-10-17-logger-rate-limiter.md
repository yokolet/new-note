---
layout: post
title: Logger Rate Limiter
hero_height: is-small
tags:
- Easy
- Hash Table
- Design
date: 2022-10-17 14:49 +0900
---
## Introduction
This logger problem is given the integer timestamp.
We don't need a specific effort about the timestamp.
How to keep the log needs a bit of idea.
The problem asks unique messaged based timestamp management.
Based on that, the solution here saves it in a hash table with the message as a key.

## Problem Description
> Design a logger system that receives a stream of messages along with their timestamps. Each unique message
> should only be printed at most every 10 seconds (i.e. a message printed at timestamp `t` will prevent other
> identical messages from being printed until timestamp `t + 10`).
>
> All messages will come in chronological order. Several messages may arrive at the same timestamp.
>
> Implement the `Logger` class:
> - `Logger()` Initializes the logger object.
> - `bool shouldPrintMessage(int timestamp, string message)` Returns true if the message should be printed in the
>    given timestamp, otherwise returns false.
>
> Constraints:
> - `0 <= timestamp <= 10**9`
> - Every `timestamp` will be passed in non-decreasing order (chronological order).
> - `1 <= message.length <= 30`
> - At most 10**4 calls will be made to `shouldPrintMessage`.
>
> [https://leetcode.com/problems/logger-rate-limiter/](https://leetcode.com/problems/logger-rate-limiter/)

## Examples
```
Example 1
Input
["Logger", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage"]
[[], [1, "foo"], [2, "bar"], [3, "foo"], [8, "bar"], [10, "foo"], [11, "foo"]]
Output
[null, true, true, false, false, false, true]

Explanation
Logger logger = new Logger();
logger.shouldPrintMessage(1, "foo");  // return true, next allowed timestamp for "foo" is 1 + 10 = 11
logger.shouldPrintMessage(2, "bar");  // return true, next allowed timestamp for "bar" is 2 + 10 = 12
logger.shouldPrintMessage(3, "foo");  // 3 < 11, return false
logger.shouldPrintMessage(8, "bar");  // 8 < 12, return false
logger.shouldPrintMessage(10, "foo"); // 10 < 11, return false
logger.shouldPrintMessage(11, "foo"); // 11 >= 11, return true, next allowed timestamp for "foo" is 11 + 10 = 21
```

## Analysis
The log is saved in the hash table, log id as a key and timestamp as a value.
When the some unique message is accessed, the timestamp value will be updated with plus 10.
This is when the message should be printed.
Next time, when the same message is accessed, compare the current and saved timestamps.
If the saved timestamp is less than the current, the logger should print it.
This is all for this logger.

## Solution
```python
class Logger:

    def __init__(self):
        self.d = {} # id: timestamp


    def shouldPrintMessage(self, timestamp: int, message: str) -> bool:
        if message not in self.d or self.d[message] <= timestamp:
            self.d[message] = timestamp + 10
            return True
        else:
            return False
```

## Complexities
- Time: `O(1)`
- Space: `O(n)`

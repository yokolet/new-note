---
layout: post
title: Logger Rate Limiter
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Hash Table
- Design
date: 2022-10-17 14:49 +0900
---

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

## How to Solve
When it comes to a design problem, the choice of a data structure is a key to solve the problem.
In this case, messages will be given with different timestamps.
Each message's timestamp different is what we should focus on.
Based on that, the hash table is a good data structure to use here.

The next to think about is how to maintain the hash table.
When a new message comes in, save message and timestamp pair in the hash table.
When an existing messages comes in, compare the timestamps.
If the previous timestamp is older more than 10 seconds, replace it with the new timestamp plus 10.
When the timestamp needs to be updated, the log should be printed.
That is all for this logger.


## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class Logger {
private:
    unordered_map<string, int> messages;  // message: timestamp
public:
    Logger() {}

    bool shouldPrintMessage(int timestamp, string message) {
        if (messages.find(message) == messages.end() || messages[message] <= timestamp) {
            messages[message] = timestamp + 10;
            return true;
        } else {
            return false;
        }
    }
};
```
{% endtab %}

{% tab solution Java %}
```java

```
{% endtab %}

{% tab solution JavaScript %}
```js

```
{% endtab %}

{% tab solution Python %}
```python
class Logger:

    def __init__(self):
        self.messages = {} # message: timestamp


    def shouldPrintMessage(self, timestamp: int, message: str) -> bool:
        if message not in self.messages or self.messages[message] <= timestamp:
            self.messages[message] = timestamp + 10
            return True
        else:
            return False
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}


## Complexities
- Time: `O(1)`
- Space: `O(n)`

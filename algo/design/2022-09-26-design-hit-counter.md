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
>
> Constraints:
> - `1 <= timestamp <= 2 * 10**9`
> - All the calls are being made to the system in chronological order (i.e., `timestamp` is monotonically increasing).
> - At most 300 calls will be made to `hit` and `getHits`.
>
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

## How to Solve
The key to solve this problem is how to maintain hits' timestamp.
Since the timestamp is monotonically increasing -- sorted already,
the binary search to find an answer looks a good approach.
However, it can be much simpler -- keep only hits from 300 seconds before.

The solution here creates a queue for timestamps.
Both, `hit` and `getHits` maintains the values in queue.
If the values are less than or equals to given timestamp - 300,
it deletes all hits before timestamp - 300.
The method, `gitHits`, returns the length of the queue. That's all.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class HitCounter {
private:
    queue<int> q;

public:
    HitCounter() {}

    void hit(int timestamp) {
        q.push(timestamp);
        while (q.front() <= timestamp - 300) {
            q.pop();
        }
    }

    int getHits(int timestamp) {
        while (!q.empty() && q.front() <= timestamp - 300) {
            q.pop();
        }
        return q.size();
    }
};
```
{% endtab %}

{% tab solution Java %}
```java
import java.util.ArrayDeque;
import java.util.Queue;

public class HitCounter {
    private Queue<Integer> q = new ArrayDeque<>();
    public HitCounter() {}

    public void hit(int timestamp) {
        q.add(timestamp);
        while (q.peek() <= timestamp - 300) {
            q.poll();
        }
    }

    public int getHits(int timestamp) {
        while (!q.isEmpty() && q.peek() <= timestamp - 300) {
            q.poll();
        }
        return q.size();
    }
}
```
{% endtab %}

{% tab solution JavaScript %}
```js
var HitCounter = function() {
    this.queue = []
};

/**
 * @param {number} timestamp
 * @return {void}
 */
HitCounter.prototype.hit = function(timestamp) {
    this.queue.push(timestamp);
    while (this.queue[0] <= timestamp - 300) {
        this.queue.shift();
    }
};

/**
 * @param {number} timestamp
 * @return {number}
 */
HitCounter.prototype.getHits = function(timestamp) {
    while (this.queue[0] <= timestamp - 300) {
        this.queue.shift();
    }
    return this.queue.length;
};
```
{% endtab %}

{% tab solution Python %}
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
{% endtab %}

{% tab solution Ruby %}
```ruby
class HitCounter
  def initialize()
    @q = Array.new
  end


=begin
    :type timestamp: Integer
    :rtype: Void
=end
  def hit(timestamp)
    @q << timestamp
    while @q[0] <= timestamp - 300
      @q.shift
    end
  end


=begin
    :type timestamp: Integer
    :rtype: Integer
=end
  def get_hits(timestamp)
    while !@q.empty? && @q[0] <= timestamp - 300
      @q.shift
    end
    @q.size
  end


end
```
{% endtab %}

{% endtabs %}


## Complexities
- Time: `O(n)` -- n: number of 300 or more older timestamps
- Space: `O(m)` -- m: number of timestamps in a queue

---
layout: post
title: Time Based Key-Value Store
hero_height: is-small
tags:
- Medium
- Binary Search
- Hash Table
- Design
date: 2022-10-06 14:33 +0900
---
## Introduction
A design problem always requires a consideration on what data structure(s) to save input data.
This problem needs a key-value store (hash table) since get method needs key access.
However, even though the same key is used, the get result depends on the timestamp.
Given that, it needs another hash table to save timestamps.
Conveniently, the timestamp is given chronologically to set method, which means it is sorted.
If the array is sorted, the binary search can be used to find the answer.
In this problem, the same index of timestamps can be used to get a value.

## Problem Description
> Design a time-based key-value data structure that can store multiple values for the same key
> at different time stamps and retrieve the key's value at a certain timestamp.
>
> Implement the TimeMap class:
> - `TimeMap()` Initializes the object of the data structure.
> - `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value` at
>    the given time `timestamp`.
> - `String get(String key, int timestamp)` Returns a value such that `set` was called previously,
>    with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value
>    associated with the largest `timestamp_prev`. If there are no values, it returns "".
>
> Constraints:
> - `1 <= key.length, value.length <= 100`
> - `key` and `value` consist of lowercase English letters and digits.
> - `1 <= timestamp <= 10**7`
> - All the timestamps `timestamp` of `set` are strictly increasing.
> - At most `2 * 10**5` calls will be made to `set` and `get`.
>
> [https://leetcode.com/problems/time-based-key-value-store/](https://leetcode.com/problems/time-based-key-value-store/)

## Examples
```
Example 1
Input
["TimeMap", "set", "get", "get", "set", "get", "get"]
[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]
Output
[null, null, "bar", "bar", null, "bar2", "bar2"]

Explanation
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
timeMap.get("foo", 1);         // return "bar"
timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.
timeMap.get("foo", 4);         // return "bar2"
timeMap.get("foo", 5);         // return "bar2"
```

```
Example 2
Input
["TimeMap","set","set","get","get","get","get","get"]
[[],["love","high",10],["love","low",20],["love",5],["love",10],["love",15],["love",20],["love",25]]
Output
[null,null,null,"","high","high","low","low"]
```

## Analysis
The solution here uses two dictionaries to save values and timestamps.
The index is the same for both values and timestamps.
When a timestamp is given to get method and whose index is i, the corresponding value's index is also i.
The timestamps associated to the key is sorted.
If the timestamp is given to get method, use the binary search to find the index in timestamps array.
The same index in values array is the answer.

## Solution
```python
class TimeMap:

    def __init__(self):
        self.values = collections.defaultdict(list)
        self.timestamps = collections.defaultdict(list)


    def set(self, key: str, value: str, timestamp: int) -> None:
        self.values[key].append(value)
        self.timestamps[key].append(timestamp)
        

    def get(self, key: str, timestamp: int) -> str:
        if key not in self.timestamps:
            return ''
        ts = self.timestamps[key]
        if not ts or timestamp < ts[0]:
            return ''
        idx = bisect.bisect_right(ts, timestamp)
        return self.values[key][idx - 1] if idx else ''
```

## Complexities
- Time: set -- `O(1)`, get -- `O(log(n))`: n is a number of timestamps associated to a key
- Space: `O(mn)`: m is a number of keys

---
layout: post
title: Design Log Storage System
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Hash Table
- String
- Design
date: 2022-10-17 14:33 +0900
---
## Introduction
When a problem is related to logging, how to handle timestamp is a key to solve the problem.
Sometime it is already an integer, float, but sometime is is given as a string.
Whether the timestamp string should be converted to specific datetime object or not is a design decision.
In this case, using it just as a string makes the code simpler.

## Problem Description
> You are given several logs, where each log contains a unique ID and timestamp. Timestamp is a string that has
> the following format: `Year:Month:Day:Hour:Minute:Second`, for example, 2017:01:01:23:59:59. All domains are
> zero-padded decimal numbers.
>
> Implement the `LogSystem` class:
> - `LogSystem()` Initializes the `LogSystem` object.
> - `void put(int id, string timestamp)` Stores the given log `(`id, timestamp)` in your storage system.
> - `int[] retrieve(string start, string end, string granularity)` Returns the IDs of the logs whose timestamps are
>    within the range from `start` to `end` inclusive. The `start` and `end` all have the same format as timestamp,
>    and `granularity` means how precise the range should be (i.e. to the exact Day, Minute, etc.). For example,
>    `start = "2017:01:01:23:59:59"`, `end = "2017:01:02:23:59:59"`, and `granularity = "Day"` means that we need
>    to find the logs within the inclusive range from Jan. 1st 2017 to Jan. 2nd 2017, and the Hour, Minute, and
>    Second for each log entry can be ignored.
>
> Constraints:
> - `1 <= id <= 500`
> - `2000 <= Year <= 2017`
> - `1 <= Month <= 12`
> - `1 <= Day <= 31`
> - `0 <= Hour <= 23`
> - `0 <= Minute, Second <= 59`
> - `granularity` is one of the values `["Year", "Month", "Day", "Hour", "Minute", "Second"]`.
> - At most 500 calls will be made to `put` and `retrieve`.
>
> [https://leetcode.com/problems/design-log-storage-system/](https://leetcode.com/problems/design-log-storage-system/)

## Examples
```
Example 1

Input
["LogSystem", "put", "put", "put", "retrieve", "retrieve"]
[[], [1, "2017:01:01:23:59:59"], [2, "2017:01:01:22:59:59"], [3, "2016:01:01:00:00:00"],
["2016:01:01:01:01:01", "2017:01:01:23:00:00", "Year"], ["2016:01:01:01:01:01", "2017:01:01:23:00:00", "Hour"]]
Output
[null, null, null, null, [3, 2, 1], [2, 1]]

Explanation
LogSystem logSystem = new LogSystem();
logSystem.put(1, "2017:01:01:23:59:59");
logSystem.put(2, "2017:01:01:22:59:59");
logSystem.put(3, "2016:01:01:00:00:00");

// return [3,2,1], because you need to return all logs between 2016 and 2017.
logSystem.retrieve("2016:01:01:01:01:01", "2017:01:01:23:00:00", "Year");

// return [2,1], because you need to return all logs between Jan. 1, 2016 01:XX:XX and Jan. 1, 2017 23:XX:XX.
// Log 3 is not returned because Jan. 1, 2016 00:00:00 comes before the start of the range.
logSystem.retrieve("2016:01:01:01:01:01", "2017:01:01:23:00:00", "Hour");
```

## Analysis
The given timestamp strings are like: 'yyyy:mm:dd:hh:MM:ss' always.
Additionally, all are zero padded.
This means comparison by string is easy.
The granularity parameter specified the length of string comparison.
If it is 'Year', the first 4 letters should be compared.
If it is 'Hour', the first 13 letters should be compared.
Given that, the solution here has a granularity to index table.
To retrieve the ids in the range, timestamp strings are compared up to the granularity index.
The answer is straightforward.

## Solution
```python
class LogSystem:

    def __init__(self):
        self.logs = []
        self.indices = {
            'Year': 5, 'Month': 8, 'Day': 11,
            'Hour': 14, 'Minute': 17, 'Second': 20
        }


    def put(self, id: int, timestamp: str) -> None:
        self.logs.append((id, timestamp))
        

    def retrieve(self, start: str, end: str, granularity: str) -> List[int]:
        idx = self.indices[granularity]
        s, e = start[:idx], end[:idx]
        return [id_ for id_, timestamp in self.logs if s <= timestamp[:idx] <= e]
```

## Complexities
- Time: put -- `O(1)`, retrieve -- `O(n)`: n is a total number of logs
- Space: `O(n)`

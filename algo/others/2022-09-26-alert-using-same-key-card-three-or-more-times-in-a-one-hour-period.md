---
layout: post
title: Alert Using Same Key-Card Three or More Times in a One Hour Period
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Hash Table
- String
- Sorting
date: 2022-09-26 14:30 +0900
---
## Introduction
The problem asks individual's 3 consecutive time differences.
Sorting is the first to be done.
We should focus on 3 consecutive time differences, so the approach would be a sliding window or two pointers.
In the end, names should be sorted to meet the requirement.

## Problem Description
> LeetCode company workers use key-cards to unlock office doors.
> Each time a worker uses their key-card, the security system saves the worker's name and the time when it was used.
> The system emits an alert if any worker uses the key-card three or more times in a one-hour period.
>
> You are given a list of strings `keyName` and `keyTime` where `[keyName[i], keyTime[i]]` corresponds to
> a person's name and the time when their key-card was used in a single day.
> Access times are given in the 24-hour time format "HH:MM", such as "23:51" and "09:49".
>
> Return a list of unique worker names who received an alert for frequent keycard use.
> Sort the names in ascending order alphabetically.
>
> Notice that "10:00" - "11:00" is considered to be within a one-hour period,
> while "22:51" - "23:52" is not considered to be within a one-hour period.
>
> Constraints:
> - `1 <= keyName.length, keyTime.length <= 10**5`
> - `keyName.length == keyTime.length`
> - `keyTime[i]` is in the format "HH:MM".
> - `[keyName[i], keyTime[i]]` is unique.
> - `1 <= keyName[i].length <= 10`
> - `keyName[i]` contains only lowercase English letters.
>
> [https://leetcode.com/problems/alert-using-same-key-card-three-or-more-times-in-a-one-hour-period/](https://leetcode.com/problems/alert-using-same-key-card-three-or-more-times-in-a-one-hour-period/)

## Examples
```
Example 1
Input: keyName = ["daniel","daniel","daniel","luis","luis","luis","luis"], keyTime = ["10:00","10:40","11:00","09:00","11:00","13:00","15:00"]
Output: ["daniel"]
Explanation: "daniel" used the keycard 3 times in a one-hour period ("10:00","10:40", "11:00").
```

```
Example 2
Input: keyName = ["alice","alice","alice","bob","bob","bob","bob"], keyTime = ["12:01","12:00","18:00","21:00","21:20","21:30","23:00"]
Output: ["bob"]
Explanation: "bob" used the keycard 3 times in a one-hour period ("21:00","21:20", "21:30").
```

## Analysis
The first step is to sort by name and time.
Then check each person's 3 consecutive time records.
If the first and last time difference is less than or equal to 60, add the name to the list.
The first of three should be popped out since next three set of time records will be used next time.
At last, sort the names as the problem requested.

## Solution
```python
class SameKeyCardAlert:
    def alertNames(self, keyName: List[str], keyTime: List[str]) -> List[str]:
        def getMins(t):
            hour, minute = t.split(':')
            return int(hour) * 60 + int(minute)
        
        workers, alerts = collections.defaultdict(list), set()
        for name, time in sorted(zip(keyName, keyTime)):
            if name in alerts:
                continue
            workers[name].append(getMins(time))
            if len(workers[name]) == 3:
                prev_t = workers[name].pop(0)
                if (workers[name][-1] - prev_t) <= 60:
                    alerts.add(name)

        return sorted(list(alerts))
```

## Complexities
- Time: `O(n)` -- n: size of input arrays
- Space: `O(m)` -- m: number of unique workers

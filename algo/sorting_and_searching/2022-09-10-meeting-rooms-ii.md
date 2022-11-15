---
layout: post
title: Meeting Rooms II -- Minimum Rooms
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Sorting
- Two Pointers
- Heap
- Array
date: 2022-09-10 19:29 +0900
---
## Introduction
Some number of problems use an array whose elements consist of start and end.
Meeting rooms, merge/insert intervals and some more are this type.
Those start from sorting the array by start or end if not yet sorted.
After that, what should be focused depends on the problem.
We should be careful to go next.

## Problem Description
> Given an array of meeting time intervals intervals
> where `intervals[i] = [start(i), end(i)]`, return the minimum number of conference rooms required.
>
> Constraints:
> - `1 <= intervals.length <= 10**4`
> - `0 <= start(i) < end(i) <= 10**6`
> 
> [https://leetcode.com/problems/meeting-rooms-ii/](https://leetcode.com/problems/meeting-rooms-ii/)

## Examples
```
Example 1:
Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2
```

```
Example 2:
Input: intervals = [[7,10],[2,4]]
Output: 1
```

## Analysis
The way of sorting is a key to solve this problem.
Like other scheduling problem, sorting the given array by start time might work.
However, it's better to sort start and end separately in this case.
When start time is smaller than end time, it needs a room since the meeting is going on.
So, count up then shift the start time.
If start time is bigger than the end time, it means previous meeting is over.
So, count down then shift the end time.

## Solution
```python
class MeetingRoomsTwo:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        n  = len(intervals)
        starts = sorted([s for s, _ in intervals])
        ends = sorted([e for _, e in intervals])
        result, cur = float('-inf'), 0
        i, j = 0, 0
        while i < n and j < n:
            if starts[i] < ends[j]:
                cur += 1
                result = max(result, cur)
                i += 1
            else:
                cur -= 1
                j += 1
        return result
```

## Complexities
- Time: `O(nlog(n))`
- Space: `O(n)`

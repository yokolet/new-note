---
layout: post
title: Divide Intervals Into Minimum Number of Groups
hero_height: is-small
tags:
- Medium
- Heap (Priority Queue)
- Greedy
- Array
date: 2022-09-21 14:44 +0900
---
## Introduction
Interval problems almost always start from sorting the given array.
Depending on the problem, the sorting is done by start time, end time or both.
In many cases, the idea of merging the interval works.
This problem doesn't need the merging, however, some sort of merging idea can be used.

## Problem Description
> You are given a 2D integer array `intervals` where `intervals[i] = [left[i], right[i]]` represents
> the inclusive interval `[left[i], right[i]]`.
>
> You have to divide the intervals into one or more groups such that each interval is in exactly one group,
> and no two intervals that are in the same group intersect each other.
> Return the minimum number of groups you need to make.
>
> Two intervals intersect if there is at least one common number between them.
> For example, the intervals `[1, 5]` and `[5, 8]` intersect.
>
> Constraints:
> - `1 <= intervals.length <= 10**5`
> - `intervals[i].length == 2`
> - `1 <= left[i] <= right[i] <= 10**6`
>
> [https://leetcode.com/problems/divide-intervals-into-minimum-number-of-groups/](https://leetcode.com/problems/divide-intervals-into-minimum-number-of-groups/)

## Examples
```
Example 1
Input: intervals = [[5,10],[6,8],[1,5],[2,3],[1,10]]
Output: 3
Explanation: We can divide the intervals into the following groups:
- Group 1: [1, 5], [6, 8].
- Group 2: [2, 3], [5, 10].
- Group 3: [1, 10].
It can be proven that it is not possible to divide the intervals into fewer than 3 groups.
```

```
Example 2
Input: intervals = [[1,3],[5,6],[8,10],[11,13]]
Output: 1
Explanation: None of the intervals overlap, so we can put all of them in one group.
```

## Analysis
The first step is to sort the given array based on the start time.
Then use heap to save the end time.
If the next start time is greater than the heap's top value, in another word, the earliest end time,
pop the earliest end time and replace it to the next end time.
If not, push the next end time since the range overlaps.
The number of final groups is the answer.

## Solution
```python
class DivideIntervalsIntoMinimumNumberOfGroups:
    def minGroups(self, intervals: List[List[int]]) -> int:
        intervals.sort()
        groups = []
        for s, e in intervals:
            if not groups:
                heapq.heappush(groups, e)
            elif groups[0] < s:
                heapq.heappop(groups)
                heapq.heappush(groups, e)
            else:
                heapq.heappush(groups, e)
        return len(groups)
```

## Complexities
- Time: `O(n*log(n))`
- Space: `O(m)` -- m: number of groups

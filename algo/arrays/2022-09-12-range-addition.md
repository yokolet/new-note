---
layout: post
title: Range Addition
hero_height: is-small
tags:
- Medium
- Prefix Sum
- Array
date: 2022-09-12 23:10 +0900
---
## Introduction
If the problem is just about an array, prefix sum might work well.
The problem may not look straightforward prefix one, but it's good to think prefix solution.

## Problem Description
> You are given an integer `length` and an array `updates` where `updates[i] = [startIdx(i), endIdx(i), inc(i)]`.
> You have an array `arr` of length `length` with all zeros, and you have some operation to apply on `arr`.
> In the i-th operation, you should increment all the elements
> `arr[startIdx(i)], arr[startIdx(i + 1)], ..., arr[endIdx(i)]` by inc(i).
>
> Return `arr` after applying all the updates.
>
> Constraints:
> - `1 <= length <= 10**5`
> - `0 <= updates.length <= 10**4`
> - `0 <= startIdx(i) <= endIdx(i) < length`
> - `-1000 <= inc(i) <= 1000`
>
> [https://leetcode.com/problems/range-addition/](https://leetcode.com/problems/range-addition/)

## Examples
```
Example 1:
Input: length = 5, updates = [[1,3,2],[2,4,3],[0,2,-2]]
Output: [-2,0,3,5,3]
```

```
Exmaple 2:
Input: length = 10, updates = [[2,4,6],[5,6,8],[1,9,-4]]
Output: [0,-4,2,2,2,4,4,-4,-4,-4]
```

## Analysis
Every value within the update range doesn't need to be updated every time.
Only start and end range is enough. To mark the end, end + 1 is decremented by inc.
After that, calculate prefix sum, which is the answer.

## Solution
```python
class RangeAddition:
    def getModifiedArray(self, length: int, updates: List[List[int]]) -> List[int]:
        prefix = [0 for _ in range(length)]
        for start, end, inc in updates:
            prefix[start] += inc
            if end < length - 1:
                prefix[end + 1] -= inc
        for i in range(1, len(prefix)):
            prefix[i] += prefix[i - 1]
        return prefix
```

## Complexities
- Time: `O(n + m)` -- n: length of updates, m: length
- Space: `O(m)`

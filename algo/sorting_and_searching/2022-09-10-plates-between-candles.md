---
layout: post
title: Plates Between Candles
hero_height: is-small
tags:
- Medium
- Binary Search
- Array
- String
date: 2022-09-10 21:44 +0900
---
## Introduction
This problem tells us to focus on only indices of `'|'`.
Then it's easy to think of a binary search solution.
The tricky part is, candles might there continuously.
Those should be subtracted.

## Problem Description
> There is a long table with a line of plates and candles arranged on top of it.
> You are given a 0-indexed string `s` consisting of characters `'*'` and `'|'` only,
> where a `'*'` represents a plate and a `'|'` represents a candle.
>
> You are also given a 0-indexed 2D integer array `queries`
> where `queries[i] = [left(i), right(i)]` denotes the substring `s[left(i)...right(i)]` (inclusive).
> For each query, you need to find the number of plates between candles that are in the substring.
> A plate is considered between candles if there is at least one candle to its left
> and at least one candle to its right in the substring.
>
> - For example, `s = "||**||**|*"`, and a `query [3, 8]` denotes the substring `"*||**|"`.
>   The number of plates between candles in this substring is 2,
>   as each of the two plates has at least one candle in the substring to its left and right.
> Return an integer array `answer` where `answer[i]` is the answer to the i-th query.
>
> Constraints:
> - `3 <= s.length <= 10**5`
> - `s` consists of `'*'` and `'|'` characters.
> - `1 <= queries.length <= 10**5`
> - `queries[i].length == 2`
> - `0 <= left(i) <= right(i) < s.length`
> 
> [https://leetcode.com/problems/plates-between-candles/](https://leetcode.com/problems/plates-between-candles/)

## Examples
```
Exmaple 1:
Input: s = "**|**|***|", queries = [[2,5],[5,9]]
Output: [2,3]
Explanation:
- queries[0] has two plates between candles.
- queries[1] has three plates between candles.
```

```
Example 2:
Input: s = "***|**|*****|**||**|*", queries = [[1,17],[4,5],[14,17],[5,11],[15,16]]
Output: [9,0,0,0,0]
Explanation:
- queries[0] has nine plates between candles.
- The other queries have zero plates between candles.
```

## Analysis
Since only indicies of candles matter, create an array of candle indicies.
Given left and right of a query, find left insertion point of left value.
Also, find right insertion point of right value.
We'll see indicies of candles, so calculate the range length.
However, the multiple candles might be there continuously, so subtract those.

## Solution
```python
class PlatesBetweenCandles:
    def platesBetweenCandles(self, s: str, queries: List[List[int]]) -> List[int]:
        nums = [i for i in range(len(s)) if s[i] == '|']
        answer = []
        for left, right in queries:
            l = bisect.bisect_left(nums, left)
            r = bisect.bisect_right(nums, right)
            if l == r:
                answer.append(0)
                continue
            idx_l, idx_r = nums[l], nums[r - 1]
            candles = r - l
            answer.append(idx_r - idx_l + 1 - candles)
        return answer
```

## Complexities
- Time: `O(mlog(n))` -- m: number of queries, n: number of candles
- Space: `O(n)`

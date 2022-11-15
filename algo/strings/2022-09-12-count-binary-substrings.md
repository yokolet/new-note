---
layout: post
title: Count Binary Substrings
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Two Pointers
- String
date: 2022-09-12 21:37 +0900
---
## Introduction
The problem asks substrings, which means sequential search is required.
If we look at the problem closely, the key to solve problem is:
how many same characters continues.

## Problem Description
> Given a binary string `s`, return the number of non-empty substrings that
> have the same number of 0's and 1's, and all the 0's
> and all the 1's in these substrings are grouped consecutively.
>
> Substrings that occur multiple times are counted the number of times they occur.
>
> Constraints:
> - `1 <= s.length <= 10**5`
> - `s[i]` is either '0' or '1'
>
> [https://leetcode.com/problems/count-binary-substrings/](https://leetcode.com/problems/count-binary-substrings/)

## Examples
```
Example 1:
Input: s = "00110011"
Output: 6
Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's:
             "0011", "01", "1100", "10", "0011", and "01".
```

```
Example 2:
Input: s = "10101"
Output: 4
Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.
```

## Analysis
Count how many same characters continue in the given string.
If the substring is `00111`, create a counts list of `[2, 3]`.
The minimum of two pairs in the counts list are the number of patters to create.

## Solution
```python
class CountBinarySubstrings:
    def countBinarySubstrings(self, s: str) -> int:
        n, counts = len(s), []
        left, right = 0, 0
        while left < n and right < n:
            while right < n and s[left] == s[right]:
                right += 1
            counts.append(right - left)
            left = right
        counts.append(right - left)
        result = 0
        for i in range(len(counts) - 1):
            result += min(counts[i], counts[i + 1])
        return result
```

## Complexities
- Time: `O(n + m)` -- n: length of string, m: length of count list
- Space: `O(m)`

---
layout: post
title: Count Unique Characters of All Substrings of a Given String
hero_height: is-small
tags:
- Hard
- Dynamic Programming
- Hash Table
- String
date: 2022-09-14 18:17 +0900
---
## Introduction
Using hash table, find a repeating character style problems are there.
This is the same type.
However, this problem asks about all substrings, so how to count the result is not easy.
Within non-repeating characters range, we should count substrings.

## Problem Description
> Let's define a function `countUniqueChars(s)` that returns the number of unique characters on `s`.
> - For example, calling `countUniqueChars(s`) if `s = "LEETCODE"`
>   then `"L", "T", "C", "O", "D"` are the unique characters since they appear only once in `s`,
>   therefore `countUniqueChars(s) = 5`.
>
> Given a string `s`, return the sum of `countUniqueChars(t)` where `t` is a substring of `s`.
> The test cases are generated such that the answer fits in a 32-bit integer.
>
> Notice that some substrings can be repeated so in this case you have to count the repeated ones too.
>
> Constraints:
> - `1 <= s.length <= 10**5`
> - `s` consists of uppercase English letters only.
>
> [https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/](https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/)

## Examples
```
Example 1:
Input: s = "ABC"
Output: 10
```

```
Example 2
Input: s = "ABA"
Output: 8
```

```
Example 3:
Input: s = "LEETCODE"
Output: 92
```

## Analysis
Start from a hash table which will keep tracking each letter's start and end indices so far.
While going over the given string, add up a count based on
the current letter's previous start and end indices.

## Solution
```python
class CountUniqueCharactersOfAllSubstringsOfAGivenString:
    def uniqueLetterString(self, s: str) -> int:
        n = len(s)
        memo = {c: (-1, -1) for c in ascii_uppercase}
        result = 0
        for idx, c in enumerate(s):
            i, j = memo[c]
            result += (j - i) * (idx - j)
            memo[c] = (j, idx)
        for c in memo:
            i, j = memo[c]
            result += (n - j) * (j - i)
        return result
```

## Complexities
- Time: `O(n)`
- Space: `O(1)`

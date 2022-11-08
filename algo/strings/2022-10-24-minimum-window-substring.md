---
layout: post
title: Minimum Window Substring
hero_height: is-small
tags:
- Hard
- Sliding Window
- Hash Table
- String
date: 2022-10-24 16:18 +0900
---
## Introduction
As the title says, this is a sliding window problem.
What makes this problem difficult is, the result substring is not just an anagram of the given pattern.
There might be extra characters between necessary characters.
The result substring length might be much longer than the pattern string.
The solution here uses Hash table to check exactly what characters are in the substring.
Also, the missing number of characters to form the pattern.
When the window slides to the right, make it increment for the leftmost character of the window.
At the same time, start and end indices are updated.

## Problem Description
> Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s`
> such that every character in `t` (including duplicates) is included in the window. If there is no such substring,
> return the empty string "".
>
> The testcases will be generated such that the answer is unique.
>
> A substring is a contiguous sequence of characters within the string.
>
> Constraints:
> - `m == s.length`
> - `n == t.length`
> - `1 <= m, n <= 10**5`
> - `s` and `t` consist of uppercase and lowercase English letters.
>
> [https://leetcode.com/problems/minimum-window-substring/](https://leetcode.com/problems/minimum-window-substring/)

## Examples
```
Example 1
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

```
Example 2
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```

```
Example 3
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

## Analysis
The solution starts from counting the pattern character and save it to the hash table.
While sliding the window, if the pattern table has enough characters of the current character,
missing count is decremented.
At this point, here's a tweak -- decrement the character in the hash table.
Python's collections.Counter is behave like collections.defaultdict(int).
If the character doesn't exist in the table, still it can be decremented.
When the missing becomes zero, all necessary characters are found.
It's time to find the start index.
For this purpose, negative counts in the table is used.
While the counts in the table is negative, the start index can be shift to the right.
Again, the counts are incremented as the pointer shifts.
When the start index is found, update the start and end indicies.
The answer is the substring from start to end indicies.

## Solution
```python
class MinimumWindowSubstring:
    def minWindow(self, s: str, t: str) -> str:
        needs, missing = collections.Counter(t), len(t)
        start, end = 0, 0
        i = 0
        for j, c in enumerate(s, 1):
            if needs[c] > 0:
                missing -= 1
            needs[c] -= 1
            if missing == 0:
                while i < j and needs[s[i]] < 0:
                    needs[s[i]] += 1
                    i += 1
                needs[s[i]] += 1
                missing += 1
                if end == 0 or end - start > j - i:
                    start, end = i, j
                i += 1
        return s[start:end]
```

## Complexities
- Time: `O(m + n)` -- m: length of s, n: length of t
- Space: `O(m + n)`

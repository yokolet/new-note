---
layout: post
title: Minimum Number of Moves to Make Palindrome
algo_menubar: algo_menu
hero_height: is-small
tags:
- Hard
- Two Pointers
- String
- Greedy
date: 2022-09-14 22:31 +0900
---
## Introduction
The problem title uses the word "move," but it is a "swap" actually.
If the problem asks about swap characters, it's good to think a two pointer solution.

## Problem Description
> You are given a string `s` consisting only of lowercase English letters.
> In one move, you can select any two adjacent characters of s and swap them.
>
> Return the minimum number of moves needed to make `s` a palindrome.
>
> Note that the input will be generated such that `s` can always be converted to a palindrome.
>
> Constraints:
> - `1 <= s.length <= 2000`
> - `s` consists only of lowercase English letters.
> - `s` can be converted to a palindrome using a finite number of moves.
>
> [https://leetcode.com/problems/minimum-number-of-moves-to-make-palindrome/](https://leetcode.com/problems/minimum-number-of-moves-to-make-palindrome/)

## Examples
```
Example 1:
Input: s = "aabb"
Output: 2
Explanation: "aabb" -> "abab" -> "abba"
```

```
Example 2:
Input: s = "letelt"
Output: 2
Explanation: "letelt" -> "letetl" -> "lettel"
```

## Analysis
Start from leftmost and rightmost characters.
If those don't match, swap right two characters until the same character to the left is found.
When it is only one character, the two pointers meet at the same index.
A single character should go to the center, so swap it to right character at the moment.
When same two characters are on the different indices, swap the right side.

## Solution
```python
class MinimumNumberOfMovesToMakePalindrome:
    def minMovesToMakePalindrome(self, s: str) -> int:
        n = len(s)
        s = list(s)
        left, right, count = 0, n - 1, 0
        while left < right:
            l, r = left, right
            while s[l] != s[r]:
                r -= 1
            if l == r:
                s[r], s[r + 1] = s[r + 1], s[r]
                count += 1
                continue
            else:
                while r < right:
                    s[r], s[r + 1] = s[r + 1], s[r]
                    r += 1
                    count += 1
            left += 1
            right -= 1
        return count
```

## Complexities
- Time: `O(n^2)`
- Space: `O(n)`
 

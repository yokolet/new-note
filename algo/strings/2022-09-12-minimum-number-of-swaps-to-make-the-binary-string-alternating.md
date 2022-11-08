---
layout: post
title: Minimum Number of Swaps to Make the Binary String Alternating
hero_height: is-small
tags:
- Medium
- String
- Greedy
date: 2022-09-12 16:10 +0900
---
## Introduction
Some of problems asks "swaps to make something."
This problem has only two kinds of letters, so greedy approach would work.

## Problem Description
> Given a binary string `s`, return the minimum number of character swaps to make it alternating,
> or -1 if it is impossible.
>
> The string is called alternating if no two adjacent characters are equal.
> For example, the strings "010" and "1010" are alternating, while the string "0100" is not.
>
> Any two characters may be swapped, even if they are not adjacent.
>
> Constraints:
> - `1 <= s.length <= 1000`
> - `s[i]` is either '0' or '1'
>
> [https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-binary-string-alternating/](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-binary-string-alternating/)

## Examples
```
Example 1:
Input: s = "111000"
Output: 1
Explanation: Swap positions 1 and 4: "111000" -> "101010"
The string is now alternating.
```

```
Example 2:
Input: s = "010"
Output: 0
Explanation: The string is already alternating, no swaps are needed.
```

```
Example 3:
Input: s = "1110"
Output: -1
```

## Analysis
At first, count zeros and ones.
The difference should be 0 or 1 to make an alternating string.
Next step is which one would be the first character.
Assuming, the first character is the one of more count,
if the current character is not the one supposed to be, count up invalid.
The `swap` function counts doubly, so it returns the value divided by 2.

## Solution
```python
class MinimumNumberOfSwapsToMakeTheBinaryStringAlternating:
    def minSwaps(self, s: str) -> int:
        def swap(first):
            invalid = 0
            for i in range(len(s)):
                if i % 2 == 0 and s[i] != first:
                    invalid += 1
                if i % 2 == 1 and s[i] == first:
                    invalid += 1
            return invalid // 2
    
        counter = collections.Counter(list(s))
        zeros, ones = counter['0'], counter['1']
        if abs(zeros - ones) > 1:
            return -1
        if zeros > ones:
            return swap('0')
        elif ones > zeros:
            return swap('1')
        else:
            return min(swap('0'), swap('1'))
```

## Complexities
- Time: `O(n)`
- Space: `O(1)`
 

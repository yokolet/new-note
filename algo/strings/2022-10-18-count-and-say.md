---
layout: post
title: Count and Say
hero_height: is-small
tags:
- Medium
- String
date: 2022-10-18 15:30 +0900
---
## Introduction
This is a relatively straightforward problem.
Just count the same character and add count + character to the result.
Both iterative and recursion work to get the final answer.

## Problem Description
> The count-and-say sequence is a sequence of digit strings defined by the recursive formula:
> - `countAndSay(1) = "1"`
> - `countAndSay(n)` is the way you would "say" the digit string from countAndSay(n-1), which is then
>    converted into a different digit string.
>
> To determine how you "say" a digit string, split it into the minimal number of substrings such that each substring
> contains exactly one unique digit. Then for each substring, say the number of digits, then say the digit. Finally,
> concatenate every said digit.
>
> For example, the saying and conversion for digit string "3322251":
> "23321511": two threes, three twos, one five, one one
>
> Given a positive integer n, return the nth term of the count-and-say sequence.
>
> Constraints:
> - `1 <= n <= 30`
>
> [https://leetcode.com/problems/count-and-say/](https://leetcode.com/problems/count-and-say/)

## Examples
```
Example 1
Input: n = 1
Output: "1"
Explanation: This is the base case.
```

```
Example 2
Input: n = 4
Output: "1211"
Explanation:
countAndSay(1) = "1"
countAndSay(2) = say "1" = one 1 = "11"
countAndSay(3) = say "11" = two 1's = "21"
countAndSay(4) = say "21" = one 2 + one 1 = "12" + "11" = "1211"
```

## Analysis
The solution here took an iterative approach.
Check the current character is the same as the previous one.
If it is the same, count up. If not, update the result and initialize parameters.
This repeats n - 1 times. Then, we get the answer.

## Solution
```python
class CountAndSay:
    def countAndSay(self, n: int) -> str:
        result = '1'
        for _ in range(n - 1):
            prev, cur, cnt = '', '', 0
            for i in range(len(result)):
                if prev and result[i] != prev:
                    cur += (str(cnt) + prev)
                    cnt = 0
                prev = result[i]
                cnt += 1
            cur += (str(cnt) + prev)
            result = cur
        return result
```

## Complexities
- Time: `O(4^(n/3))`
- Space: `O(4^(n/3))`

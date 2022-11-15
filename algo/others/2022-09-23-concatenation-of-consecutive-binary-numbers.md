---
layout: post
title: Concatenation of Consecutive Binary Numbers
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Bit Manipulation
- Simulation
- Math
date: 2022-09-23 14:50 +0900
---
## Introduction
The idea of this problem is not difficult.
The previous result gets bit-shifted by the bits of current number, then add the current number.
However, the problem mentions `modulo 10**9 + 7`, which alerts we need to do something more.
There are a couple of approaches such as converting to string problem or bitwise operation.

## Problem Description
> Given an integer `n`, return the decimal value of the binary string formed by concatenating
> the binary representations of `1` to `n` in order, `modulo 10**9 + 7`.
>
> Constraints:
> - `1 <= n <= 10**5`
>
> [https://leetcode.com/problems/concatenation-of-consecutive-binary-numbers/](https://leetcode.com/problems/concatenation-of-consecutive-binary-numbers/)

## Examples
```
Example 1
Input: n = 1
Output: 1
Explanation: "1" in binary corresponds to the decimal value 1. 
```

```
Example 2
Input: n = 3
Output: 27
Explanation: In binary, 1, 2, and 3 corresponds to "1", "10", and "11".
After concatenating them, we have "11011", which corresponds to the decimal value 27.
```

```
Example 3
Input: n = 12
Output: 505379714
```

## Analysis
The solution here uses bitwise operation.
There are to keys to solve this problem effectively.
One is to know how many bit shift is necessary.
Bit shift size changes when x & (x - 1) = 0.
For example, 11 to 100, 111 to 1000, 1111 to 10000, ...
Given that, when x & (x - 1) = 0, shift size is incremented.
Another is the addition.
All bits for current value i are zero since the previous value is shifted to left for the bit size of the current value.
In this case, OR operator works as the addition.
This way, we get the answer.

## Solution
```python
class ConcatenationOfConsecutiveBinaryNumbers:
    def concatenatedBinary(self, n: int) -> int:
        mod = 10 ** 9 + 7
        result, shift = 0, 0
        for i in range(1, n + 1):
            if i & (i - 1) == 0:
                shift += 1
            result = (result << shift | i) % mod
        return result
```

## Complexities
- Time: `O(n)`
- Space: `O(1)`

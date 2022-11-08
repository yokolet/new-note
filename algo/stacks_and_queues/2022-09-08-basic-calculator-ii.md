---
layout: post
title: Basic Calculator II
hero_height: is-small
tags:
- Medium
- Stack
- String
- Math
date: 2022-09-08 14:38 +0900
---
## Introduction
This is the simplest basic calculator problem.
The given string doesn't include parentheses, so the precedence handling is straightforward.
The problem can be solved using a stack which saves values.

## Problem Description
> Given a string `s` which represents an expression, evaluate this expression and return its value.
> The integer division should truncate toward zero.
>
> You may assume that the given expression is always valid.
> All intermediate results will be in the range of `[-2**31, 2**31 - 1]`.
>
> You are not allowed to use any built-in function which evaluates
> strings as mathematical expressions, such as `eval()`.
>
> Constraints:
> - `1 <= s.length <= 3 * 10**5`
> - `s` consists of integers and operators `('+', '-', '*', '/')` separated by some
>     number of spaces.
> - `s` represents a valid expression.
> - All the integers in the expression are non-negative integers in the range `[0, 2**31 - 1]`.
> - The answer is guaranteed to fit in a 32-bit integer.
>
> [https://leetcode.com/problems/basic-calculator-ii/](https://leetcode.com/problems/basic-calculator-ii/)

## Examples
```
Example 1:
Input: s = "3+2*2"
Output: 7
```

```
Example 2:
Input: s = " 3/2 "
Output: 1
```

```
Example 3:
Input: s = " 3+5 / 2 "
Output: 5
```

## Analysis
We should handle just 4 types of operators.
Apparently, those operators need 2 values,
so the process goes one step behind compared to currently looking at operator.
It starts from saving current digits in a variable.
When one of the operator is found, perform previous operator. Its default is `+`.
At the very first operator in the string,
saves the digits in the stack and operator in the variable.
The second operator pops out the previous value from the stack and apply previous operator.
The calculation result is pushed to the stack.

In the end, take sum of values in the stack, which is the answer.

## Solution
```python
class BasicCalculatorTwo:
    def calculate(self, s: str) -> int:
        s = s.replace(' ', '')
        stack, digits, op = [], '', '+'
        for i in range(len(s)):
            c = s[i]
            if c not in '+-*/':
                digits += c
            if c in '+-*/' or i == len(s) - 1:
                if op == '+':
                    stack.append(int(digits))
                elif op == '-':
                    stack.append((-1) * int(digits))
                elif op == '*':
                    x = stack.pop()
                    stack.append(x * int(digits))
                elif op == '/':
                    x = stack.pop()
                    stack.append(int(x / int(digits)))
                digits, op = '', c
        return sum(stack)
```

## Complexities
- Time: `O(n)`
- Space: `O(n)`
 

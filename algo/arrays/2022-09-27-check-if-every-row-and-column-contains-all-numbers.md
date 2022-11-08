---
layout: post
title: Check if Every Row and Column Contains All Numbers
hero_height: is-small
tags:
- Easy
- Matrix
- Array
date: 2022-09-27 16:03 +0900
---
## Introduction
The problem is about n x n matrix and 1 to n (inclusive) values.
Using a set data structure, check cells of each row and columns doesn't duplicate.

## Problem Description
> An `n x n` matrix is valid if every row and every column contains all the integers from 1 to n (inclusive).
>
> Given an `n x n` integer matrix `matrix`, return `true` if the matrix is valid. Otherwise, return `false`.
>
> Constraints:
> - `n == matrix.length == matrix[i].length`
> - `1 <= n <= 100`
> - `1 <= matrix[i][j] <= n`
>
> [https://leetcode.com/problems/check-if-every-row-and-column-contains-all-numbers/](https://leetcode.com/problems/check-if-every-row-and-column-contains-all-numbers/)

## Examples
```
Example 1
Input: matrix = [[1,2,3],[3,1,2],[2,3,1]]
Output: true
```

```
Example 2
Input: matrix = [[1,1,1],[1,2,3],[1,2,3]]
Output: false
```

## Analysis
The solution uses a set for horizontal and array of sets for vertical.
When a row is scanned, it checks the horizontal set.
When all rows are scanned, it checks the array of vertical sets.
If all are length n, return True.

## Solution
```python
class CheckAllNumbers:
    def checkValid(self, matrix: List[List[int]]) -> bool:
        n = len(matrix)
        vertical = [set() for _ in range(n)]
        for i in range(n):
            horizontal = set()
            for j in range(n):
                cur = matrix[i][j]
                horizontal.add(cur)
                vertical[j].add(cur)
            if len(horizontal) != n:
                return False
        for i in range(n):
            if len(vertical[i]) != n:
                return False
        return True
```

## Complexities
- Time: `O(n^2)`
- Space: `O(n^2)`

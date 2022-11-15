---
layout: post
title: Where Will the Ball Fall
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Depth-First Search
- Simulation
- Matrix
date: 2022-11-01 22:03 +0900
---
## Introduction
At a glance, this problem looks weird.
However, it is relatively simple problem solved by the depth-first search.
When two adjacent cell has the same value (same direction of slanted boards), the ball goes through.
Otherwise, the ball gets stuck.
From row to row, the ball goes left and right by the value of the cell.
When the value is negative, the ball goes to left. If it is positive, the ball goes to right.
When the row comes to the bottom, return the column where the ball is located.

## Problem Description
> You have a 2-D grid of size `m x n` representing a box, and you have `n` balls. The box is open on the top and
> bottom sides. Each cell in the box has a diagonal board spanning two corners of the cell that can redirect a ball to
> the right or to the left.
> - A board that redirects the ball to the right spans the top-left corner to the bottom-right corner and is
>   represented in the grid as 1.
> - A board that redirects the ball to the left spans the top-right corner to the bottom-left corner and is represented
>   in the grid as -1.
>
> We drop one ball at the top of each column of the box. Each ball can get stuck in the box or fall out of the bottom.
> A ball gets stuck if it hits a "V" shaped pattern between two boards or if a board redirects the ball into either
> wall of the box.
>
> Return an array answer of size `n` where `answer[i]` is the column that the ball falls out of at the bottom after
> dropping the ball from the ith column at the top, or -1 if the ball gets stuck in the box.
>
> Constraints:
> - `m == grid.length`
> - `n == grid[i].length`
> - `1 <= m, n <= 100`
> - `grid[i][j]` is 1 or -1.
>
> [https://leetcode.com/problems/where-will-the-ball-fall/](https://leetcode.com/problems/where-will-the-ball-fall/)

## Examples
```
Example 1
Input: grid = [[1,1,1,-1,-1],[1,1,1,-1,-1],[-1,-1,-1,1,1],[1,1,1,1,-1],[-1,-1,-1,-1,-1]]
Output: [1,-1,-1,-1,-1]
Explanation:
Ball b0 is dropped at column 0 and falls out of the box at column 1.
Ball b1 is dropped at column 1 and will get stuck in the box between column 2 and 3 and row 1.
Ball b2 is dropped at column 2 and will get stuck on the box between column 2 and 3 and row 0.
Ball b3 is dropped at column 3 and will get stuck on the box between column 2 and 3 and row 0.
Ball b4 is dropped at column 4 and will get stuck on the box between column 2 and 3 and row 1.
```

```
Example 2
Input: grid = [[-1]]
Output: [-1]
Explanation: The ball gets stuck against the left wall.
```

```
Example 3
Input: grid = [[1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1],[1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1]]
Output: [0,1,2,3,4,-1]
```

## Analysis
To make the ball to go through all rows, the grid needs 2 columns at least.
So, if the grid column size is 1, return an array of -1.
In the dfs function, the base case is the current row becomes the grid size.
Return column when it hits the base case.
If not, get the new column value by simply adding the grid value.
Conveniently, the grid values are defined like that.
If the new column goes outside of the grid or adjacent grid values are not the same, return -1.
If all are ok, go deeper with the next row and new column.


## Solution
```python
class WhereWillTheBallFall:
    def findBall(self, grid: List[List[int]]) -> List[int]:
        m, n = len(grid), len(grid[0])
        if n == 1:
            return [-1]
        result = []
        def dfs(r, c):
            if r == m:
                return c
            nc = c + grid[r][c]
            if nc < 0 or nc > n - 1 or grid[r][c] != grid[r][nc]:
                return -1
            return dfs(r + 1, nc)
        for i in range(n):
            result.append(dfs(0, i))
        return result
```

## Complexities
- Time: `O(m*n)`
- Space: `O(m)`

---
layout: post
title: Shortest Path in a Grid with Obstacles Elimination
hero_height: is-small
tags:
- Hard
- Breadth-First Search
- Matrix
- Array
date: 2022-10-30 17:06 +0900
---
## Introduction
If this problem doesn't have a condition, "at most k obstacles elimination,"
it is a typical breadth-first search problem.
Now, the condition of obstacles elimination has been added.
Still, the way of solving the problem stays the same.
Instead of adding (next_row, next_col) to the queue, it adds (next_row next_col, number_of_k_left).
The number of k left will be decreased by one if the grid is the obstacle, otherwise keep the same value.
While going over the cells, if the cell is the obstacle and k is 0, the cell won't be added to the queue.
This way, we can find the answer.

## Problem Description
> You are given an `m x n` integer matrix grid where each cell is either 0 (empty) or 1 (obstacle). You can move up,
> down, left, or right from and to an empty cell in one step.
>
> Return the minimum number of steps to walk from the upper left corner `(0, 0)` to the lower right corner
> `(m - 1, n - 1)` given that you can eliminate at most `k` obstacles. If it is not possible to find such walk return -1.
>
> Constraints:
> - `m == grid.length`
> - `n == grid[i].length`
> - `1 <= m, n <= 40`
> - `1 <= k <= m * n`
> - `grid[i][j]` is either 0 or 1.
> - `grid[0][0] == grid[m - 1][n - 1] == 0`
>
> [https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

## Examples
```
Example 1
Input: grid = [[0,0,0],[1,1,0],[0,0,0],[0,1,1],[0,0,0]], k = 1
Output: 6
Explanation: 
The shortest path without eliminating any obstacle is 10.
The shortest path with one obstacle elimination at position (3,2) is 6. Such path is
(0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2) -> (3,2) -> (4,2).
```

```
Example 2
Input: grid = [[0,1,1],[1,1,1],[1,0,0]], k = 1
Output: -1
Explanation: We need to eliminate at least two obstacles to find such a walk.
```

## Analysis
The problem has an extra condition, however, traversing cells stays the same.
The algorithm is the breadth-first search since the problem asks the shortest path.
The queue will have the combination of (distance, (next_row, next_col, number_of_k_left)).
The the cell is the obstacle, the number of k left is decreased by one.
Conveniently, the obstacle value is 1, so k - grid[row][col] does the decrement.
When the popped out row and col is the one of right bottom, return the distance.

## Solution
```python
class ShortestPathInAGridWithObstaclesElimination:
    def shortestPath(self, grid: List[List[int]], k: int) -> int:
        rows, cols = len(grid), len(grid[0])
        queue = [(0, (0, 0, k))] # dist, (row, col, k)
        seen = set((0, 0, k))
        dirs = [[-1, 0], [0, -1], [0, 1], [1, 0]]
        while queue:
            dist, (r, c, k) = queue.pop(0)
            if (r, c) == (rows - 1, cols - 1):
                return dist
            for rr, cc in dirs:
                next_r, next_c = r + rr, c + cc
                if next_r < 0 or next_r >= rows or\
                    next_c < 0 or next_c >= cols or\
                    (grid[next_r][next_c] == 1 and k == 0) or\
                    (next_r, next_c, k) in seen:
                    continue
                seen.add((next_r, next_c, k))
                queue.append((dist + 1, (next_r, next_c, k - grid[next_r][next_c])))
        return -1
```

## Complexities
- Time: `O(n*k)` -- n: number of cells in the grid, k: obstacle eliminate limit
- Space: `O(n*k)`

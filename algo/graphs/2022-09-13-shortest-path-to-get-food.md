---
layout: post
title: Shortest Path to Get Food
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Breadth-First Search
- Matrix
date: 2022-09-13 22:17 +0900
---
## Introduction
The breadth-first search is the way to reach to the target quickly.
Also, a greedy approach by Dijkstra works well.

## Problem Description
> You are starving and you want to eat food as quickly as possible.
> You want to find the shortest path to arrive at any food cell.
>
> You are given an `m x n` character matrix, `grid`, of these different types of cells:
> - `'*'` is your location. There is exactly one `'*'` cell.
> - `'#'` is a food cell. There may be multiple food cells.
> - `'O'` is free space, and you can travel through these cells.
> - `'X'` is an obstacle, and you cannot travel through these cells.
>
> You can travel to any adjacent cell north, east, south, or west of your current location
> if there is not an obstacle.
>
> Return the length of the shortest path for you to reach any food cell.
> If there is no path for you to reach food, return -1.
>
> Constraints:
> - `m == grid.length`
> - `n == grid[i].length`
> - `1 <= m, n <= 200`
> - `grid[row][col]` is `'*', 'X', 'O', or '#'`.
> - The grid contains exactly one `'*'`.
>
> [https://leetcode.com/problems/shortest-path-to-get-food/](https://leetcode.com/problems/shortest-path-to-get-food/)

## Examples
```
Example 1:
Input: grid = [["X","X","X","X","X","X"],["X","*","O","O","O","X"],["X","O","O","#","O","X"],["X","X","X","X","X","X"]]
Output: 3
Explanation: It takes 3 steps to reach the food.
```

```
Example 2:
Input: grid = [["X","X","X","X","X"],["X","*","X","O","X"],["X","O","X","#","X"],["X","X","X","X","X"]]
Output: -1
Explanation: It is not possible to reach the food.
```

```
Example 3:
Input: grid = [["X","X","X","X","X","X","X","X"],["X","*","O","X","O","#","O","X"],["X","O","O","X","O","O","X","X"],["X","O","O","O","O","#","O","X"],["X","X","X","X","X","X","X","X"]]
Output: 6
Explanation: There can be multiple food cells. It only takes 6 steps to reach the bottom food.
```

## Analysis
The first step is to find a starting cell.
This needs to go over all cells.
Once the starting cell is found, the solution uses the Dijkstra to get to the target quickly.
The value to push the heap is a tuple of (distance, row, col).
It maintains the minimum distance so far at visited cells.
When a popped cell position has a value `'#''`, the food is found.

## Solution
```python
class ShortestPathToGetFood:
    def getFood(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])

        def findStart():
            for i in range(m):
                for j in range(n):
                    if grid[i][j] == '*':
                        return (i, j)
        
        x, y = findStart()
        queue = [(0, x, y)]
        visited = {(x, y): 0}
        while queue:
            dist, row, col = heapq.heappop(queue)
            if grid[row][col] == '#':
                return dist
            for r, c in [(row - 1, col), (row, col - 1), (row, col + 1), (row + 1, col)]:
                if 0 <= r < m and 0 <= c < n and \
                grid[r][c] != 'X' and \
                ((r, c) not in visited or visited[(r, c)] > dist + 1):
                    heapq.heappush(queue, (dist + 1, r, c))
                    visited[(r, c)] = dist + 1
        return -1

```

## Complexities
- Time: `O(m * n + klog(k))` -- m: rows, n: columns, k: number of free spaces
- Space: `O(k)`

---
layout: post
title: Number of Islands
hero_height: is-small
tags:
- Medium
- Depth-First Search
- Union-Find
- Matrix
date: 2022-09-10 14:57 +0900
---
## Introduction
This is a matrix search problem. Possible movements are four: up, right, down, and left.
To get the answer, we should look at an area of `1`s.
Given that, Depth-First or Breadth-First search are the typical algorithm to solve the problem.
This type of problem can have one more way: union-find.
The solution here has DFS and union-find.

## Problem Description
> Given an `m x n` 2D binary grid `grid` which represents a map of '1's (land) and '0's (water),
> return the number of islands.

> An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically.
> You may assume all four edges of the grid are all surrounded by water.
>
> Constraints:
> - `m == grid.length`
> - `n == grid[i].length`
> - `1 <= m, n <= 300`
> - `grid[i][j]` is '0' or '1'.
>
> [https://leetcode.com/problems/number-of-islands/](https://leetcode.com/problems/number-of-islands/)

## Examples
```
Example 1:
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

```
Example 2:
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

## Analysis
The solution by DFS is a common DFS search.
Starting from all positions of `1`, check up, right, down, left if those are '1' and not yet visited.
When the DSF starts, the counter is incremented since only one `1` is an island.

On the other hand, the solution by union-find saves the group number in an auxiliary array.
It checks group id of up and left cells and updates current cell's group id.
In the end, the number of groups is calculated going over the group id array.

## Solution
```python
class NumberOfIslandsDFS:
    def numIslands(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        visited = set()
        def dfs(r, c):
            visited.add((r, c))
            for next_r, next_c in [(r - 1, c), (r, c - 1), (r, c + 1), (r + 1, c)]:
                if 0 <= next_r < m and 0 <= next_c < n and\
                    grid[next_r][next_c] == '1' and\
                    (next_r, next_c) not in visited:
                    dfs(next_r, next_c)
        count = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1' and (i, j) not in visited:
                    count += 1
                    dfs(i, j)
        return count
```

```python
class NumberOfIslandsUF:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid or not grid[0]: return 0

        cols = len(grid[0])
        memo = [0 for _ in range(cols)]
        groups = [0]
        gno = 1
        for row in grid:
            for i in range(cols):
                if row[i] == '0':
                    memo[i] = 0
                    continue
                up = groups[memo[i]]
                tmp = 0 if i == 0 else memo[i - 1]
                left = groups[tmp]
                if up == 0 and left == 0:
                    memo[i] = gno
                    groups.append(gno)
                    gno += 1
                elif up == 0:
                    memo[i] = left
                elif left == 0:
                    memo[i] = up
                elif up > left:
                    groups[up] = left
                    memo[i] = left
                else:
                    # up < left
                    groups[left] = up
                    memo[i] = up
        count = 0
        for i in range(1, len(groups)):
            if i == groups[i]:
                count += 1
        return count
```

## Complexities
- Time: `O(m * n)` -- m: rows, n: columns
- Space: `O(m * n)` -- DFS, `O(m)` -- UF 

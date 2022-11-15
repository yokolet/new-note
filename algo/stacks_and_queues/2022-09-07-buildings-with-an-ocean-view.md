---
layout: post
title: Buildings With an Ocean View
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Monotonic Stack
date: 2022-09-07 17:30 +0900
---
## Introduction

At a glance, it looks a sorting problem. However, this is a monotonic stack problem.

## Problem Description
> There are `n` buildings in a line.
> You are given an integer array `heights` of size `n` that represents the heights of the buildings in the line.
>
> The ocean is to the right of the buildings.
> A building has an ocean view if the building can see the ocean without obstructions.
> Formally, a building has an ocean view if all the buildings to its right have a smaller height.
>
> Return a list of indices (0-indexed) of buildings that have an ocean view,
> sorted in increasing order.
>
> Constraints:
> - `1 <= heights.length <= 10**5`
> - `1 <= heights[i] <= 10**9`
>
> [https://leetcode.com/problems/buildings-with-an-ocean-view/](https://leetcode.com/problems/buildings-with-an-ocean-view/)

## Examples
```
Example 1:
Input: heights = [4,2,3,1]
Output: [0,2,3]
Explanation: Building 1 (0-indexed) does not have an ocean view because building 2 is taller.
```

```
Example 2:
Input: heights = [4,3,2,1]
Output: [0,1,2,3]
Explanation: All the buildings have an ocean view.
```

```
Example 3:
Input: heights = [1,3,2,4]
Output: [3]
Explanation: Only building 3 has an ocean view.
```

## Analysis

If we keep monotonically decreasing stack, the solution is not complicated.
The values in the stack are indices.
Staring from the value of index 0, keep popping or pushing indices one by one
to keep monotonically decreasing stack index.
The stack itself is the answer.

## Solution
```python
class BuildingWithAnOceanView:
    def findBuildings(self, heights: List[int]) -> List[int]:
        if not heights:
            return []
        n, stack = len(heights), []
        stack.append(0)
        for i in range(1, n):
            while stack and heights[stack[-1]] <= heights[i]:
                stack.pop()
            stack.append(i)
        return stack
```

## Complexities
- Time: `O(n)`
- Space: `O(n)`

---
layout: post
title: Maximum Units on a Truck
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Sorting
- Greedy
date: 2022-09-13 14:13 +0900
---
## Introduction
This problem might look like dynamic programming.
However, the input array's order does not  matter.
Sorting and greedy approach works.

## Problem Description
> You are assigned to put some amount of boxes onto one truck.
> You are given a 2D array `boxTypes`, where `boxTypes[i] = [numberOfBoxes(i), numberOfUnitsPerBox(i)]`:
> - `numberOfBoxes(i)` is the number of boxes of type `i`.
> - `numberOfUnitsPerBox(i)` is the number of units in each box of the type `i`.
>
> You are also given an integer `truckSize`, which is the maximum number of boxes that can be put on the truck.
> You can choose any boxes to put on the truck as long as the number of boxes does not exceed `truckSize`.
> Return the maximum total number of units that can be put on the truck.
>
> Constraints:
> - `1 <= boxTypes.length <= 1000`
> - `1 <= numberOfBoxes(i), numberOfUnitsPerBox(i) <= 1000`
> - `1 <= truckSize <= 10**6`
>
> [https://leetcode.com/problems/maximum-units-on-a-truck/](https://leetcode.com/problems/maximum-units-on-a-truck/)

## Examples
```
Example 1:
Input: boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
Output: 8
Explanation: There are:
- 1 box of the first type that contains 3 units.
- 2 boxes of the second type that contain 2 units each.
- 3 boxes of the third type that contain 1 unit each.
You can take all the boxes of the first and second types, and one box of the third type.
The total number of units will be = (1 * 3) + (2 * 2) + (1 * 1) = 8.
```

```
Example 2:
Input: boxTypes = [[5,10],[2,5],[4,7],[3,9]], truckSize = 10
Output: 91
```

## Analysis
For the first step, sort `boxTypes` array by units.
The second step is to go over the sorted `boxTypes` array.
Possible number of boxes to put on the truck is minimum of boxes and current truckSize.
Add up possible number of boxes times current units.
If the truckSize becomes zero, a loop is over.

## Solution
```python
class MaximumUnitsOnATruck:
    def maximumUnits(self, boxTypes: List[List[int]], truckSize: int) -> int:
        boxTypes.sort(key=lambda x: x[1], reverse=True)
        amount = 0
        for b, u in boxTypes:
            cur = min(b, truckSize)
            amount += cur * u
            truckSize -= cur
            if truckSize == 0:
                break
        return amount
```

## Complexities
- Time: `O(nlong(n))`
- Space: `O(1)`
 

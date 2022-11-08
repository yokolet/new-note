---
layout: post
title: Cheapest Flights Within K Stops
hero_height: is-small
tags:
- Medium
- Heap (Priority Queue)
- Breadth-First Search
- Graph
date: 2022-10-21 23:08 +0900
---
## Introduction
The problem asks the shortest path based on an edge weight.
Given that, Dijkstra is a good algorithm to find an answer.
As for graph traversal, the visited state needs a tweak.
The solution here uses an array to save visited times.
If the vertex is visited too many times, further process will be skipped.

## Problem Description
> There are `n` cities connected by some number of flights. You are given an array `flights` where
> `flights[i] = [from[i], to[i], price[i]]` indicates that there is a flight from city `from[i]` to city toi with cost
> `price[i]`.
>
> You are also given three integers `src`, `dst`, and `k`, return the cheapest price from `src` to `dst` with
> at most `k` stops. If there is no such route, return -1.
>
> Constraints:
> - 1 <= n <= 100`
> - `0 <= flights.length <= (n * (n - 1) / 2)`
> - `flights[i].length == 3`
> - `0 <= from[i], to[i] < n`
> - `from[i] != to[i]`
> - `1 <= price[i] <= 10**4`
> - There will not be any multiple flights between two cities.
> - `0 <= src, dst, k < n`
> - `src != dst`
>
> [tps://leetcode.com/problems/cheapest-flights-within-k-stops/](tps://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Examples
```
Example 1

Input: n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
Output: 700
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 3 is marked in red and has cost 100 + 600 = 700.
Note that the path through cities [0,1,2,3] is cheaper but is invalid because it uses 2 stops.
```

```
Exampoe 2

Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 2 is marked in red and has cost 100 + 100 = 200.
```

```
Example 3

Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
Output: 500
Explanation:
The graph is shown above.
The optimal path with no stops from city 0 to 2 is marked in red and has cost 500.
```

## Analysis
The first step is to create a directed graph with edge weights.
The solution here uses a tuple of (node, weight).
The visited is not a set, instead an array.
While traversing, if the visited count exceeds the currently visiting vertex, skip the further process.
Other than that, the traversal is common one.
When the popped out vertex is the destination, return the weight.

## Solution
```python
class CheapestFlightsWithinKStops:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        graph = collections.defaultdict(list)
        
        # build a graph
        for s, d, w in flights:
            graph[s].append((d, w))

        # traverse a graph
        visited = [0 for _ in range(n)]
        queue = [(0, k + 1, src)]    # weight, stops, vertex
        while queue:
            cur_w, cur_s, cur_n = heapq.heappop(queue)
            if cur_n == dst:
                return cur_w
            if visited[cur_n] >= cur_s:
                continue
            visited[cur_n] = cur_s
            for next_n, next_w in graph[cur_n]:
                heapq.heappush(queue, (cur_w + next_w, cur_s - 1, next_n))
        return -1
```

## Complexities
- Time: `O(n + e * log(n))` -- n: number of vertices, e: number of edges
- Space: `O(n + e)`

---
layout: post
title: Network Delay Time
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Heap (Priority Queue)
- Breadth-First Search
- Shortest Path
- Graph
date: 2022-10-21 20:59 +0900
---
## Introduction
This problem can be solved using simple DFS or BFS.
However, Dijkstra works well.
Additionally, the problem needs a data structure to save a sum of weight (delay time) to each node.
Among those weight, maximum is the answer since Dijkstra finds shortest paths.

## Problem Description
> You are given a network of `n` nodes, labeled from `1 to n`. You are also given `times`, a list of travel times as
> directed edges `times[i] = (u[i], v[i], w[i])`, where `u[i]` is the source node, `v[i]` is the target node, and
> `w[i]` is the time it takes for a signal to travel from source to target.
>
> We will send a signal from a given node `k`. Return the minimum time it takes for all the `n` nodes to receive the
> signal. If it is impossible for all the `n` nodes to receive the signal, return -1.
>
> Constraints:
> - `1 <= k <= n <= 100`
> - `1 <= times.length <= 6000`
> - `times[i].length == 3`
> - `1 <= u[i], v[i] <= n`
> - `u[i] != v[i]`
> - `0 <= w[i] <= 100`
> - All the pairs `(u[i], v[i])` are unique. (i.e., no multiple edges.)
>
> [https://leetcode.com/problems/network-delay-time/](https://leetcode.com/problems/network-delay-time/)

## Examples
```
Exmaple 1
Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
Output: 2
```

```
Example 2
Input: times = [[1,2,1]], n = 2, k = 1
Output: 1
```

```
Example 3
Input: times = [[1,2,1]], n = 2, k = 2
Output: -1
```

## Analysis
The first step is to create a graph with edge weights.
The second step is to traverse a graph using Dijkstra algorithm.
The edge weight sum is a sorting key. The queue (heap) has tuples of weight sum and node.
While traversing, it saves the shortest path's sum of weights to each node.
In the end, the maximum of weights is the minimum time delay.

## Solution
```python
class NetworkDelayTime:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        graph = collections.defaultdict(list)
        def buildGraph():
            for s, d, w in times:
                graph[s].append((d, w))

        path = {}
        def traverse():
            queue = [(0, k)]
            while queue:
                cur_w, cur_n = heapq.heappop(queue)
                if cur_n in path:
                    continue
                path[cur_n] = cur_w
                for next_n, next_w in graph[cur_n]:
                    if next_n not in path:
                        heapq.heappush(queue, (cur_w + next_w, next_n))

        buildGraph()
        traverse()
        return max(path.values()) if len(path) == n else -1
```

## Complexities
- Time: `O(n + e * log(n))` -- n: number of vertices, e: number of edges
- Space: `O(n + e)`

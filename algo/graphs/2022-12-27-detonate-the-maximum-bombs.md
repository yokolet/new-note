---
layout: post
title: Detonate the Maximum Bombs
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Graph
- Breadth-First Search
- Geometry
date: 2022-12-27 15:05 +0900
---
## Problem Description
> You are given a list of bombs. The range of a bomb is defined as the area where its effect can be felt. This area
> is in the shape of a circle with the center as the location of the bomb.
>
> The bombs are represented by a 0-indexed 2D integer array `bombs` where `bombs[i] = [x[i], y[i], r[i]]`. `x[i]` and
> `y[i]` denote the X-coordinate and Y-coordinate of the location of the ith bomb, whereas `r[i]` denotes the radius
> of its range.
>
> You may choose to detonate a single bomb. When a bomb is detonated, it will detonate all bombs that lie in its range.
> These bombs will further detonate the bombs that lie in their ranges.
>
> Given the list of `bombs`, return the maximum number of bombs that can be detonated if you are allowed to detonate
> only one bomb.
>
> Constraints:
> - `1 <= bombs.length <= 100`
> - `bombs[i].length == 3`
> - `1 <= x[i], y[i], r[i] <= 10**5`
>
> [https://leetcode.com/problems/detonate-the-maximum-bombs/](https://leetcode.com/problems/detonate-the-maximum-bombs/)

## Examples
```
Example 1
Input: bombs = [[2,1,3],[6,1,4]]
Output: 2
Explanation:
The above figure shows the positions and ranges of the 2 bombs.
If we detonate the left bomb, the right bomb will not be affected.
But if we detonate the right bomb, both bombs will be detonated.
So the maximum bombs that can be detonated is max(1, 2) = 2.
```

```
Example 2
Input: bombs = [[1,1,5],[10,10,5]]
Output: 1
Explanation:
Detonating either bomb will not detonate the other bomb, so the maximum number of bombs that can be detonated is 1.
```

```
Example 3
Input: bombs = [[1,2,3],[2,3,1],[3,4,2],[4,5,3],[5,6,4]]
Output: 5
Explanation:
The best bomb to detonate is bomb 0 because:
- Bomb 0 detonates bombs 1 and 2. The red circle denotes the range of bomb 0.
- Bomb 2 detonates bomb 3. The blue circle denotes the range of bomb 2.
- Bomb 3 detonates bomb 4. The green circle denotes the range of bomb 3.
Thus all 5 bombs are detonated.
```

## How to Solve
It's easy to think of the solution to count all points within each circle.
If the radius is r, center is x0, y0, then r^2 >= (x - x0)^2 + (y - y0)^2 is a formula to check other points are
within this circle or not.
However, the problem introduced another bar of detonating one by one.
Because of that, simple counting doesn't work well.
To represent the detonating situation, a graph data structure is used here.
In the graph, each node (center) has adjacent nodes (neighboring centers) if the distance is less than or equals to
the radius.
Once the graph is constructed, do the breadth-first search (BFS) starting from each node (index) while counting up.
The maximum count is the answer.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class DetonateTheMaximumBombs {
public:
    int maximumDetonation(vector<vector<int>>& bombs) {
        auto isNeighbor = [](const vector<int> &a, const vector<int> &b) {
            long long dx = a[0] - b[0], dy = a[1] - b[1];
            long long r = a[2];
            return r * r >= dx * dx + dy * dy;
        };
        int n = bombs.size();
        // build a graph
        vector<vector<int>> graph(n);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i == j) { continue; }
                if (isNeighbor(bombs[i], bombs[j])) {
                    graph[i].push_back(j);
                }
            }
        }
        vector<int> counts(n, 1);
        // bfs
        for (int i = 0; i < n; ++i) {
            queue<int> q({i});
            unordered_set<int> seen({i});
            while (!q.empty()) {
                vector<int> neighbors = graph[q.front()];
                q.pop();
                for (int neigh : neighbors) {
                    if (seen.find(neigh) == seen.end()) {
                        counts[i]++;
                        seen.insert(neigh);
                        q.push(neigh);
                    }
                }
            }
        }
        return *max_element(counts.begin(), counts.end());
    }
};
```
{% endtab %}

{% tab solution Java %}
```java

```
{% endtab %}

{% tab solution JavaScript %}
```js

```
{% endtab %}

{% tab solution Python %}
```python
class DetonateTheMaximumBombs:
    def maximumDetonation(self, bombs: List[List[int]]) -> int:
        def isNeighbor(a: List[int], b: List[int]) -> bool:
            dx, dy = a[0] - b[0], a[1] - b[1]
            return a[2] * a[2] >= dx * dx + dy * dy

        n = len(bombs)
        # build a graph
        graph = [[] for _ in range(n)]
        for i in range(n):
            for j in range(n):
                if i == j: continue
                if isNeighbor(bombs[i], bombs[j]):
                    graph[i].append(j)
        counts = [1 for _ in range(n)]
        # bfs
        for i in range(n):
            queue, seen = [i], set({i})
            while queue:
                neighbors = graph[queue.pop(0)]
                for neigh in neighbors:
                    if neigh not in seen:
                        counts[i] += 1
                        seen.add(neigh)
                        queue.append(neigh)
        return max(counts)
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(n * n)`
- Space: `O(n)`

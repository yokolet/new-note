---
layout: post
title: The Earliest Moment When Everyone Become Friends
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Union Find
- Array
date: 2022-12-20 13:28 +0900
---
## Problem Description
> There are `n` people in a social group labeled from `0` to `n - 1`. You are given an array logs where
> `logs[i] = [timestamp[i], x[i], y[i]]` indicates that `x[i]` and `y[i]` will be friends at the time `timestamp[i]`.
>
> Friendship is symmetric. That means if a is friends with b, then b is friends with a. Also, person a is acquainted
> with a person b if a is friends with b, or a is a friend of someone acquainted with b.
>
> Return the earliest time for which every person became acquainted with every other person. If there is no such
> earliest time, return -1.
>
> Constraints:
> - `2 <= n <= 100`
> - `1 <= logs.length <= 10**4`
> - `logs[i].length == 3`
> - `0 <= timestampi <= 10**9`
> - `0 <= x[i], y[i] <= n - 1`
> - `x[i] != y[i]`
> - All the values `timestamp[i]` are unique.
> - All the pairs `(x[i], y[i])` occur at most one time in the input.
>
> [https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/](https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/)

## Examples
```
Example 1
Input: logs = [[20190101,0,1],[20190104,3,4],[20190107,2,3],[20190211,1,5],[20190224,2,4],[20190301,0,3],
              [20190312,1,2],[20190322,4,5]]
       n = 6
Output: 20190301
Explanation:
20190101: [0, 1], [2], [3], [4], [5]
20190104: [0, 1], [2], [3, 4], [5]
20190107: [0, 1], [2, 3, 4], [5]
20190211: [0, 1, 5], [2, 3, 4]
20190224: [0, 1, 5], [2, 3, 4]
20190301: [0, 1, 5, 2, 3, 4]
```

```
Example 2
Input: logs = [[0,2,0],[1,0,1],[3,0,3],[4,1,2],[7,3,1]], n = 4
Output: 3
Explanation: At timestamp = 3, all the persons (i.e., 0, 1, 2, and 3) become friends.
```

## How to Solve
It's not difficult to think of the union-find approach for this problem.
At the beginning, n groups are there.
As time goes by, groups are merged and merged.
Eventually, a single group will be formed.
This is exactly the union-find does.

Ths solution here starts from sorting the given logs by the timestamp.
After that, the solution uses a typical union-find approach.
When the number of groups becomes 1, the answer is there.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class Solution {
private:
    vector<int> groups;

public:
    int _find(int a) {
        while (a != groups[a]) {
            a = groups[a];
        }
        return a;
    }

    int _union(int a, int b, int n) {
        int ga = _find(a);
        int gb = _find(b);
        if (ga != gb) {
            groups[ga] = gb;
            n -= 1;
        }
        return n;
    }

    int earliestAcq(vector<vector<int>>& logs, int n) {
        for (auto i = 0; i < n; ++i) {
            groups.push_back(i);
        }
        sort(logs.begin(), logs.end(), [](const auto &a, const auto &b) {return a[0] < b[0]; });
        for (vector<int> log : logs) {
            int t = log[0], a = log[1], b = log[2];
            n = _union(a, b, n);
            if (n == 1) { return t; }
        }
        return -1;
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
class Solution:
    def earliestAcq(self, logs: List[List[int]], n: int) -> int:
        groups = [i for i in range(n)]

        def find(a):
            while a != groups[a]:
                a = groups[a]
            return a

        def union(a, b, n):
            ga, gb = find(a), find(b)
            if ga != gb:
                groups[ga] = gb
                n -= 1
            return n

        for t, a, b in sorted(logs, key=lambda x: x[0]):
            n = union(a, b, n)
            if n == 1:
                return t
        return -1
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(log(n))`
- Space: `O(n)`

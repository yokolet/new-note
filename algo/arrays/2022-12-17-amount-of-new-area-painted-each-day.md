---
layout: post
title: Amount of New Area Painted Each Day
algo_menubar: algo_menu
hero_height: is-small
tags:
- Hard
- Hash Table
- Array
date: 2022-12-17 21:38 +0900
---
## Problem Description
> There is a long and thin painting that can be represented by a number line. You are given a 0-indexed 2D integer
> array paint of length `n`, where `paint[i] = [start[i], end[i]]`. This means that on the i-th day you need to paint
> the area between `start[i]` and `end[i]`.
>
> Painting the same area multiple times will create an uneven painting so you only want to paint each area of the
> painting at most once.
>
> Return an integer array `worklog` of length `n`, where `worklog[i]` is the amount of new area that you painted on
> the i-th day.
>
> Constraints:
> - `1 <= paint.length <= 10**5`
> - `paint[i].length == 2`
> - `0 <= start[i] < end[i] <= 5 * 10**4`
>
> [https://leetcode.com/problems/amount-of-new-area-painted-each-day/](https://leetcode.com/problems/amount-of-new-area-painted-each-day/)

## Examples
```
Exmaple 1
nput: paint = [[1,4],[4,7],[5,8]]
Output: [3,3,1]
Explanation:
On day 0, paint everything between 1 and 4.
The amount of new area painted on day 0 is 4 - 1 = 3.
On day 1, paint everything between 4 and 7.
The amount of new area painted on day 1 is 7 - 4 = 3.
On day 2, paint everything between 7 and 8.
Everything between 5 and 7 was already painted on day 1.
The amount of new area painted on day 2 is 8 - 7 = 1.
```

```
Example 2
Input: paint = [[1,4],[5,8],[4,7]]
Output: [3,3,1]
Explanation:
On day 0, paint everything between 1 and 4.
The amount of new area painted on day 0 is 4 - 1 = 3.
On day 1, paint everything between 5 and 8.
The amount of new area painted on day 1 is 8 - 5 = 3.
On day 2, paint everything between 4 and 5.
Everything between 5 and 7 was already painted on day 1.
The amount of new area painted on day 2 is 5 - 4 = 1.
```

```
Example 3
Input: paint = [[1,5],[2,4]]
Output: [4,0]
Explanation:
On day 0, paint everything between 1 and 5.
The amount of new area painted on day 0 is 5 - 1 = 4.
On day 1, paint nothing because everything between 2 and 4 was already painted on day 0.
The amount of new area painted on day 1 is 0.
```

## How to Solve
As this problem is categorized to the hard, it is a hard problem.
Easy solution such that all range checks will get the time limit exceeded error.
The problem requires an effective solution.
Various approaches are there to solve this problem,
for example, interval merges, binary search, ordered set, segment tree and more.

Among those, the solution here took the hash table approach.
If the given start and end range doesn't exist in the hash table,
save all values from start to end (end is not included) will be added to the hash table
with the end as a value.
If the given start and end range overlaps to the existing range(s),
update the end value by a bigger one.
Also, skip the check to the original end value.

In the inner loop, the start value shifts one by one, so the work will grow one by one.
When the all values between start and end are checked, add the work to the worklog array.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class AmountOfNewAreaPaintedEachDay {
public:
    vector<int> amountPainted(vector<vector<int>>& paint) {
        unordered_map<int, int> painted;
        vector<int> worklog;
        for (size_t i = 0; i < paint.size(); ++i) {
            int start = paint[i][0], end = paint[i][1];
            int work = 0;
            while (start < end) {
                if (painted.find(start) != painted.end()) {
                    int prev_end = painted[start];
                    painted[start] = max(painted[start], end);
                    start = prev_end;
                } else {
                    painted[start] = end;
                    start++;
                    work++;
                }
            }
            worklog.push_back(work);
        }
        return worklog;
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
class AmountOfNewAreaPaintedEachDay:
    def amountPainted(self, paint: List[List[int]]) -> List[int]:
        painted = {}
        worklog = []
        for start, end in paint:
            work = 0
            while start < end:
                if start in painted:
                    prev_end = painted[start]
                    painted[start] = max(painted[start], end)
                    start = prev_end
                else:
                    painted[start] = end
                    start += 1
                    work += 1
            worklog.append(work)
        return worklog

```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(m * n)` -- m: length of paint, n: range size in the pain array
- Space: `O(m * n)`

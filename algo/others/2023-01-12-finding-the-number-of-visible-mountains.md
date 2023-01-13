---
layout: post
title: Finding the Number of Visible Mountains
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Math
- Sorting
date: 2023-01-12 17:44 +0900
---
## Problem Description
> You are given a 0-indexed 2D integer array `peaks` where `peaks[i] = [x[i], y[i]]` states that mountain `i` has a
> peak at coordinates `(x[i], y[i])`. A mountain can be described as a right-angled isosceles triangle, with its base
> along the x-axis and a right angle at its peak. More formally, the gradients of ascending and descending the
> mountain are 1 and -1 respectively.
>
> A mountain is considered visible if its peak does not lie within another mountain (including the border of other
> mountains).
>
> Return the number of visible mountains.
>
> Constraints:
> - `1 <= peaks.length <= 10**5`
> - `peaks[i].length == 2`
> - `1 <= x[i], y[i] <= 10**5`
>
> [https://leetcode.com/problems/finding-the-number-of-visible-mountains/](https://leetcode.com/problems/finding-the-number-of-visible-mountains/)

## Examples
```
Example 1
Input: peaks = [[2,2],[6,3],[5,4]]
Output: 2
Explanation: The diagram above shows the mountains.
- Mountain 0 is visible since its peak does not lie within another mountain or its sides.
- Mountain 1 is not visible since its peak lies within the side of mountain 2.
- Mountain 2 is visible since its peak does not lie within another mountain or its sides.
There are 2 mountains that are visible.
```

```
Example 2
Input: peaks = [[1,3],[1,3]]
Output: 0
Explanation: The diagram above shows the mountains (they completely overlap).
Both mountains are not visible since their peaks lie within each other.

```

## How to Solve
From the description that the triangle is a right-angled isosceles triangle, two slopes are: y = x + b and y = -x + b
where b is a y intercept.
We can easily find two y intercepts as y - x and y + x.
Positions of triangles are separated, partially overlap, or included.
For the first step, sort y intercept pairs: (y - x, y + x) in a descending order, which means the triangles are
arranged from left most to right most.
Then, check one by one whether the upper y intercept is greater than the current max value.
However, if the peaks overlap, those should not be counted.
Update the max value if it is a bigger y intercept.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class FindingTheNumberOfVisibleMountains {
public:
    int visibleMountains(vector<vector<int>>& peaks) {
        for (int i = 0; i < peaks.size(); ++i) {
            int x = peaks[i][0], y = peaks[i][1];
            peaks[i][0] = y - x, peaks[i][1] = y + x;
        }
        sort(peaks.begin(), peaks.end(),
            [](const vector<int> &a, const vector<int> &b) -> bool {
                return a[0] == b[0] ? a[1] > b[1] : a[0] > b[0];
            });
        int result = 0, max_v = 0, n = peaks.size();
        for (int i = 0; i < n; ++i) {
            if (peaks[i][1] > max_v) {
                if (i == n - 1 || peaks[i][0] != peaks[i + 1][0] || peaks[i][1] != peaks[i + 1][1]) {
                    result++;
                }
                max_v = peaks[i][1];
            }
        }
        return result;
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
class FindingTheNumberOfVisibleMountains:
    def visibleMountains(self, peaks: List[List[int]]) -> int:
        peaks = [(y - x, y + x) for x, y in peaks]
        peaks.sort(reverse=True)
        n, max_v, result = len(peaks), 0, 0
        for i in range(n):
            if  peaks[i][1] > max_v:
                if i == n - 1 or peaks[i] != peaks[i + 1]:
                    result += 1
                max_v = peaks[i][1]
        return result
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(n * log(n))`
- Space: `O(1)`

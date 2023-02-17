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

## How to Solve

At a glance, it looks a sorting problem, however this can be solved by a monotonic stack.
The values in the stack are indices.
Create a monotonically decreasing stack from the given array since the ocean is on the right, back of the given array.
Staring from index 0, repeat popping or pushing indices one by one to keep monotonically decreasing index stack.
When all values in the given array is checked, the stack itself is the answer.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
#include <vector>

using namespace std;

class BuildingWithAnOceanView {
public:
    vector<int> findBuildings(vector<int>& heights) {
        vector<int> indices{0};
        for (int i = 1; i < heights.size(); ++i) {
            while (!indices.empty() && heights[indices.back()] <= heights[i]) {
                indices.pop_back();
            }
            indices.push_back(i);
        }
        return indices;
    }
};
```
{% endtab %}

{% tab solution Java %}
```java
import java.util.ArrayList;
import java.util.List;

class BuildingWithAnOceanView {
    public int[] findBuildings(int[] heights) {
        List<Integer> indices = new ArrayList<Integer>();
        indices.add(0);
        for (int i = 1; i < heights.length; ++i) {
            while (!indices.isEmpty() && heights[indices.get(indices.size() - 1)] <= heights[i]) {
                indices.remove(indices.size() - 1);
            }
            indices.add(i);
        }
        int[] result = new int[indices.size()];
        for (int i = 0; i < indices.size(); ++i) { result[i] = indices.get(i); }
        return result;
    }
}
```
{% endtab %}

{% tab solution JavaScript %}
```js
/**
 * @param {number[]} heights
 * @return {number[]}
 */
var findBuildings = function(heights) {
  const indices = [0];
  for (let i = 1; i < heights.length; i++) {
    while (indices.length > 0 && heights[indices.slice(-1)] <= heights[i]) {
      indices.pop();
    }
    indices.push(i);
  }
  return indices;
};
```
{% endtab %}

{% tab solution Python %}
```python
from typing import List

class BuildingWithAnOceanView:
    def findBuildings(self, heights: List[int]) -> List[int]:
        n, stack = len(heights), []
        stack.append(0)
        for i in range(1, n):
            while stack and heights[stack[-1]] <= heights[i]:
                stack.pop()
            stack.append(i)
        return stack
```
{% endtab %}

{% tab solution Ruby %}
```ruby
# @param {Integer[]} heights
# @return {Integer[]}
def find_buildings(heights)
    indices = [0]
    (1...heights.size).each do |i|
        while !indices.empty? && heights[indices[-1]] <= heights[i]
            indices.pop
        end
        indices << i
    end
    return indices
end
```
{% endtab %}

{% endtabs %}


## Complexities
- Time: `O(n)`
- Space: `O(n)`

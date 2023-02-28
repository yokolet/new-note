---
layout: post
title: Minimize Maximum -- Divide Chocolate
date: 2022-07-22 23:30 +0900
algo_menubar: algo_menu
hero_height: is-small
tags: [Hard, Binary Search, Array]
---

## Problem Description
> You have one chocolate bar that consists of some chunks. Each chunk has its own sweetness given by the array sweetness.
>
> You want to share the chocolate with your k friends so you start cutting the chocolate bar into k + 1 pieces using k cuts,
> each piece consists of some consecutive chunks.
> 
> Being generous, you will eat the piece with the minimum total sweetness and give the other pieces to your friends.
>
> Find the maximum total sweetness of the piece you can get by cutting the chocolate bar optimally.
> 
> [https://leetcode.com/problems/divide-chocolate/](https://leetcode.com/problems/divide-chocolate/)

## Examples
```
example 1:
Input: sweetness = [1,2,3,4,5,6,7,8,9], k = 5
Answer: 6
Explanation: sweetness subarrays after k cuts (k + 1 groups): [[1,2,3],[4,5],[6],[7],[8],[9]], sums: [6,9,6,7,8,9]
6 is the minimized sum when the array is splitted into 6 chunks
```
```
example 2:
Input: sweetness = [5,6,7,8,9,1,2,3,4], k = 8
Answer: 1
Explanation: sweetness subarrays after k cuts (k + 1 groups):: [[5],[6],[7],[8],[9],[1],[2],[3],[4]], sums: [5,6,7,8,9,1,2,3,4]
1 is the minimized largest sum when the array is splitted into 9 groups
```
```
example 3:
Input: sweetness = [1,2,2,1,2,2,1,2,2], k = 2
Answer: 5
Explanation: sweetness subarrays after k cuts (k + 1 groups):: [[1,2,2],[1,2,2],[1,2,2]] sums: [5,5,5]
5 is the minimized largest sum when the array is splitted into 3 groups
```

## How to Solve
This is one of minimize-maximum algorithm problems.
This problem has an apparent variation compared to the basic problem of splitting array.
At a glance, the problem looks something else.
However, if we carefully read the description, we realize the problem is the type of minimize-maximum.

The given sweetness array should be divided into groups (k cuts + 1) based on a certain criteria.
In this case, the criteria is the subarray sum.
However, unlike splitting array problem, the middle value calculation, a way of updating left/right values and
some more parameter handling are different.
The optimal subarray sum lies between the smallest value in the array (left) and total sum of the array (right).
The iteration starts off from the middle of those left and right values.
The middle value is set `(left + right + 1) / 2` since the criteria should be set where it exceeds the exact middle value.
The array will be split base on the comparison between the sum so far and the middle value.
When the subarray sum becomes bigger than the middle value, it forms the group.
In the end, some number of subarrays will be created.

Next step is to adjust the left or right values.
If the number of subarrays is less than the specified value, the middle value should be smaller to create more subarrays.
If the number of subarrays is greater than the specified value, the middle value should be greater to create less subarrays.
Here's another variation. When the left value is updated, it will have the middle value instead of middle + 1.

When the binary search is over, we will get the answer.


## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
#include <numeric>
#include <vector>
using namespace std;

class DivideChocolate {
private:
    bool check(vector<int> &sweetness, int k, int mid) {
        int count = 0, cur = 0;
        for (int i = 0; i < sweetness.size(); ++i) {
            cur += sweetness[i];
            if (cur >= mid) {
                cur = 0;
                ++count;
            }
        }
        if (cur >= mid) // for the uncounted last bits
            ++count;
        return count < k + 1;
    }

public:
    int maximizeSweetness(vector<int>& sweetness, int k) {
        int left = *min_element(sweetness.begin(), sweetness.end());
        int right = accumulate(sweetness.begin(), sweetness.end(), 0);
        while (left < right) {
            int mid = (left + right + 1) / 2;
            if (check(sweetness, k, mid)) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        return left;
    }
};
```
{% endtab %}

{% tab solution Java %}
```java
import java.util.Arrays;

public class DivideChocolate {
    private boolean check(int[] sweetness, int k, int mid) {
        int count = 0, cur = 0;
        for (int v : sweetness) {
            cur += v;
            if (cur >= mid) {
                cur = 0;
                ++count;
            }
        }
        if (cur >= mid) {
            ++count;
        }
        return count < k + 1;
    }
    public int maximizeSweetness(int[] sweetness, int k) {
        int left = Arrays.stream(sweetness).min().getAsInt();
        int right = Arrays.stream(sweetness).sum();
        while (left < right) {
            int mid = (left + right + 1) / 2;
            if (check(sweetness, k, mid)) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        return left;
    }
}
```
{% endtab %}

{% tab solution JavaScript %}
```js
/**
 * @param {number[]} sweetness
 * @param {number} k
 * @return {number}
 */
var maximizeSweetness = function(sweetness, k) {
  const check = (mid) => {
    let count = 0, cur = 0;
    for (const v of sweetness) {
      cur += v;
      if (cur >= mid) {
        cur = 0;
        count += 1;
      }
    }
    if (cur >= mid) {
      count += 1;
    }
    return count < k + 1;
  }
  let left = Math.min(...sweetness)
  let right = sweetness.reduce((acc, v) => acc + v, 0);
  while (left < right) {
    let mid = Math.floor((left + right + 1) / 2);
    if (check(mid)) {
      right = mid - 1;
    } else {
      left = mid;
    }
  }
  return left;
};
```
{% endtab %}

{% tab solution Python %}
```python
from typing import List

class DivideChocolate:
    def maximizeSweetness(self, sweetness: List[int], k: int) -> int:
        def check(mid):
            count, cur = 0, 0
            for v in sweetness:
                cur += v
                if cur >= mid:
                    cur = 0
                    count += 1
            if cur >= mid:
                count += 1
            return count < k + 1

        left, right = min(sweetness), sum(sweetness)
        while left < right:
            mid = (left + right + 1) // 2
            if check(mid):
                right = mid - 1
            else:
                left = mid
        return left
```
{% endtab %}

{% tab solution Ruby %}
```ruby
# @param {Integer[]} sweetness
# @param {Integer} k
# @return {Integer}
def maximize_sweetness(sweetness, k)
  check = lambda do |mid|
    count, cur = 0, 0
    sweetness.each do |v|
      cur += v
      if cur >= mid
        cur = 0
        count += 1
      end
    end
    if cur >= mid
      count += 1
    end
    count < k + 1
  end

  left, right = sweetness.min, sweetness.sum
  while left < right
    mid = (left + right + 1) / 2
    if check.call(mid)
      right = mid - 1
    else
      left = mid
    end
  end
  left
end
```
{% endtab %}

{% endtabs %}


## Complexities
- Time: O(n log(s)) – s: sum of elements in the array
- Space: O(1)

---
layout: post
title: Minimize Maximum -- Cutting Ribbons
date: 2022-07-24 16:30 +0900
algo_menubar: algo_menu
hero_height: is-small
tags: [Medium, Binary Search, Array]
---

## Problem Description
> You are given an integer array ribbons, where `ribbons[i]` represents the length of the i-th ribbon, and an integer k.
> You may cut any of the ribbons into any number of segments of positive integer lengths, or perform no cuts at all.
> 
> Your goal is to obtain k ribbons of all the same positive integer length.
> You are allowed to throw away any excess ribbon as a result of cutting.
>
> Return the maximum possible positive integer length that you can obtain k ribbons of, or 0
> if you cannot obtain k ribbons of the same length.
> 
> Example of cutting ribbons of length 4
> - use as is
> - cut into 3 and 1
> - cut into 2 and 2
> - cut into 2, 1 and 1
> - cut into 1, 1, 1 and 1
> 
> [https://leetcode.com/problems/cutting-ribbons/](https://leetcode.com/problems/cutting-ribbons/)

## Examples
```
example 1:
Input: ribbons = [9,7,5], k = 3
Answer: 5
Explanation: ribbons subarrays after cutting each element: [[5,4],[5,2],[5]].
5 is the minimized largest length when the array is splitted.
```
```
example 2:
Input: ribbons = [7,5,9], k = 4
Answer: 4
Explanation: ribbons subarrays after cutting each element: [[4,3],[4,1],[4,4,1]].
4 is the minimized largest length when the array is splitted.
```
```
example 3:
Input: ribbons = [5,7,9], k = 22
Answer: 0
Explanation: sum of all ribbons are 21. Even the shortest length of 1 won't make 22 ribbons.
```


## How to Solve
This is one of minimize-maximum algorithm problems.
This problem has a variation compared to the basic, splitting array, problem.
If we look around other minimize maximum problems, it is very close to the divide chocolate problem.
Even though this has further variation, we can solve using the same approach.

Each element of the given ribbons array should be divided into smaller lengths based on a certain criteria.
The criteria is a total number of divided ribbons.
It can be found by counting the number of ribbons in subarrays.

So, we will count how many ribbons are cut out based on some value (middle value).
The smallest value (left) can be 0, and the largest (right) would be the sum of all ribbons in the given array.
Here, we should be careful to choose the starting right value.
Since the sum might goes to very large, sometime, it triggers a runtime error.
Because of this reason, the solution chose `100010` as a big enough number.
The middle value is set to `(left + right + 1) / 2` since the length of each ribbon should exceed the exact middle value.

Next step is to adjust the left or right values.
If the number of ribbons is less than the specified value, the middle value should be smaller to create more ribbons.
If the number of ribbons is greater than the specified value, the middle value should be greater to create less ribbons.
Here's a bit of variation in updating left value.
When it is updated, the left will have the middle value instead of middle + 1.

When the binary search is over, we can find the answer.


## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
#include <vector>
using namespace std;

class CuttingRibbons
{
private:
  bool check(vector<int> &ribbons, int k, int mid)
  {
    int count = 0;
    for (int i = 0; i < ribbons.size(); ++i)
    {
      count += ribbons[i] / mid;
    }
    return count < k;
  }
public:
  int maxLength(vector<int> &ribbons, int k)
  {
    int left = 0, right = 100010;  // this is big enough to get a correct answer
    while (left < right)
    {
      int mid = (left + right + 1) / 2;
      if (check(ribbons, k, mid))
      {
        right = mid - 1;
      }
      else
      {
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
class CuttingRibbons {
    private boolean check(int[] ribbons, int k, int mid) {
        int count = 0;
        for (int i = 0; i < ribbons.length; ++i) {
            count += ribbons[i] / mid;
        }
        return count < k;
    }

    public int maxLength(int[] ribbons, int k) {
        int left = 0, right = 100010;
        while (left < right) {
            int mid = (left + right + 1) / 2;
            if (check(ribbons, k, mid)) {
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
 * @param {number[]} ribbons
 * @param {number} k
 * @return {number}
 */
var maxLength = function(ribbons, k) {
  const check = (mid) => {
    let count = 0;
    for (let i = 0; i < ribbons.length; i++) {
      count += Math.floor(ribbons[i] / mid);
    }
    return count < k;
  };

  let left = 0, right = 100010, mid;
  while (left < right) {
    mid = Math.floor((left + right + 1) / 2);
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

class CuttingRibbons:
    def maxLength(self, ribbons: List[int], k: int) -> int:
        def check(mid):
            count = 0
            for v in ribbons:
                count += v // mid
            return count < k

        left, right = 0, 100010
        while left < right:
            mid = (left + right + 1) // 2
            count = 0
            if check(mid):
                right = mid - 1
            else:
                left = mid
        return left
```
{% endtab %}

{% tab solution Ruby %}
```ruby
# @param {Integer[]} ribbons
# @param {Integer} k
# @return {Integer}
def max_length(ribbons, k)
  check = lambda do |mid|
    count = 0
    ribbons.each do |v|
      count += v / mid
    end
    count < k
  end
  left, right = 0, 100010
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
- Time: O(n log(s)) – s: difference between 1 and max value in the given array.
- Space: O(1)

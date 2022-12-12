---
layout: post
title: Climbing Stairs
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Dynamic Programming
- Memoization
date: 2022-12-12 14:21 +0900
---
## Problem Description
> You are climbing a staircase. It takes `n` steps to reach the top.
>
> Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
>
> Constraints:
> - `1 <= n <= 45`
>
> [https://leetcode.com/problems/climbing-stairs/](https://leetcode.com/problems/climbing-stairs/)

## Examples
```
Example 1
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

```
Example 2
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

## How to Solve

This is a well-known problem.
The DP (dynamic programming) and recursion are common approaches.
The DP approach can simplify the solution by considering the problem as a fibonacci sequence.
The sequence is: step(n) = step(n - 1) + step(n - 2).
Starting from two states, previous of previous and previous, repeat adding those gives the answer.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class ClimbingStairs {
public:
    int climbStairs(int n) {
        if (n <= 2) { return n; }
        int prevprev = 1, prev = 2, cur = 0;
        for (auto i = 2; i < n; ++i) {
            cur = prevprev + prev;
            prevprev = prev;
            prev = cur;
        }
        return cur;
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
class ClimbingStairs:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n
        prevprev = 1
        prev = 2
        cur = 0
        for i in range(2, n):
            cur = prevprev + prev
            prevprev, prev = prev, cur
        return cur
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}


## Complexities
- Time: `O(n)`
- Space: `O(1)`

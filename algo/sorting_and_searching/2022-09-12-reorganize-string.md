---
layout: post
title: Reorganize String
hero_height: is-small
tags:
- Medium
- Heap
- String
- Greedy
date: 2022-09-12 18:03 +0900
---
## Introduction
The problem asks to return a re-generated string, so, just count the character frequency is not enough.
Since reorder is required, it needs some sorting as well.
Using heap, generate a new string based on the character frequency is the way.

## Problem Description
> Given a string `s`, rearrange the characters of `s` so that any two adjacent characters are not the same.
> Return any possible rearrangement of `s` or return `""` if not possible.
>
> Constraints:
> - `1 <= s.length <= 500`
> - s consists of lowercase English letters.
>
> [https://leetcode.com/problems/reorganize-string/](https://leetcode.com/problems/reorganize-string/)

## Examples
```
Example 1:
Input: s = "aab"
Output: "aba"
```

```
Example 2:
Input: s = "aaab"
Output: ""
```

## Analysis
The first step is to create a heap based on the character frequency.
It is the max heap, so frequency values are negative for Python heap.
Take the most frequent character and add it to the result and count down the number of the character.
Still, the character's count is more than 0, push it back to the heap with decremented count.
If the last character in the result string is the same as the most frequent, take the second frequent character.
Do the same, add to the result, get it back to heap if necessary.
This way, the result string will be created.

## Solution
```python
class ReorganizeString:
    def reorganizeString(self, s: str) -> str:
        counter = collections.Counter(list(s))
        heap = []
        for k, v in counter.items():
            heapq.heappush(heap, (-v, k))
        result = ''
        while heap:
            cnt, c = heapq.heappop(heap)
            if result and result[-1] == c:
                if not heap:
                    return ''
                cnt2, c2 = heapq.heappop(heap)
                result += c2
                cnt2 += 1
                if cnt2:
                    heapq.heappush(heap, (cnt2, c2))
            else:
                result += c
                cnt += 1
            if cnt:
                heapq.heappush(heap, (cnt, c))
        return result
```

## Complexities
- Time: `O(nlog(n))`
- Space: `O(n)`

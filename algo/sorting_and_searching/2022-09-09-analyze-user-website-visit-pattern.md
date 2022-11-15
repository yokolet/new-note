---
layout: post
title: Analyze User Website Visit Pattern
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Sorting
- Hash Table
date: 2022-09-09 17:30 +0900
---
## Introduction
This problem needs much reading.
Once we can figure out, we'll find it needs multiple sorts.
Additionally, creating combinations of three websites is required.
Python has a built-in function to create the combinations.
That function is used here.

## Problem Description
> You are given two string arrays username and website and an integer array timestamp.
> All the given arrays are of the same length and the tuple
> `[username[i], website[i], timestamp[i]]` indicates that
> the user `username[i]` visited the website `website[i]` at time `timestamp[i]`.
>
> A pattern is a list of three websites (not necessarily distinct).
> For example, ["home", "away", "love"], ["leetcode", "love", "leetcode"],
> and ["luffy", "luffy", "luffy"] are all patterns.
>
> The score of a pattern is the number of users that visited all the websites
> in the pattern in the same order they appeared in the pattern.
>
> Return the pattern with the largest score.
> If there is more than one pattern with the same largest score,
> return the lexicographically smallest such pattern.
>
> Constraints:
> - `3 <= username.length <= 50`
> - `1 <= username[i].length <= 10`
> - `timestamp.length == username.length`
> - `1 <= timestamp[i] <= 10**9`
> - `website.length == username.length`
> - `1 <= website[i].length <= 10`
> - `username[i]` and `website[i]` consist of lowercase English letters.
> - It is guaranteed that there is at least one user who visited at least three websites.
> - All the tuples `[username[i], timestamp[i], website[i]]` are unique.
>
> [https://leetcode.com/problems/analyze-user-website-visit-pattern/](https://leetcode.com/problems/analyze-user-website-visit-pattern/)

## Examples
```
Example 1:
Input: username = ["joe","joe","joe","james","james","james","james","mary","mary","mary"],
       timestamp = [1,2,3,4,5,6,7,8,9,10],
       website = ["home","about","career","home","cart","maps","home","home","about","career"]
Output: ["home","about","career"]
Explanation: The tuples in this example are:
["joe","home",1],["joe","about",2],["joe","career",3],["james","home",4],["james","cart",5],["james","maps",6],["james","home",7],["mary","home",8],["mary","about",9], and ["mary","career",10].
The pattern ("home", "about", "career") has score 2 (joe and mary).
The pattern ("home", "cart", "maps") has score 1 (james).
The pattern ("home", "cart", "home") has score 1 (james).
The pattern ("home", "maps", "home") has score 1 (james).
The pattern ("cart", "maps", "home") has score 1 (james).
The pattern ("home", "home", "home") has score 0 (no user visited home 3 times).
```

```
Example 2:
Input: username = ["ua","ua","ua","ub","ub","ub"],
       timestamp = [1,2,3,4,5,6],
       website = ["a","b","a","a","b","c"]
Output: ["a","b","a"]
```

## Analysis
The solution consists of three steps.
The first step is to create a user based website list sorted by timestamp.
This should be saved in a hash table.
The second step is to create three websites combinations from the values of the first  hash table.
While creating the three website combination, count up that.
The last step is to find out the most visited 3 websites combination from the second hash table.

## Solution
```python
class AnalyzeUserWebsiteVisitPattern:
    def mostVisitedPattern(self, username: List[str], timestamp: List[int], website: List[str]) -> List[str]:
        n = len(username)
        visits = defaultdict(list)
        for t, u, w in sorted(zip(timestamp, username, website), key=lambda x: x[0]):
            visits[u].append(w)
        scores = defaultdict(int)
        for ws in visits.values():
            for comb in set(itertools.combinations(ws, 3)):
                scores[comb] += 1
        max_score, pat = 0, None
        for k, v in scores.items():
            if max_score < v:
                max_score = v
                pat = k
            elif max_score == v:
                pat = min(pat, k)
        return list(pat)
```

## Complexities
- Time: `O(n*log(n))` -- sorting, `O(n^3)` -- combinations
- Space: `O(n+m)` -- n: number of users, m: number of 3 website combinations

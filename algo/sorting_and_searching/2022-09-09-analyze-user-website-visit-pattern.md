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

## How to Solve
This problem needs much reading.
The website visit history should be saved in individual user's list sorted by timestamp.
Additionally, creating combinations of three websites is required.
Python has a built-in function to create the combinations, so the function is used for the Python solution.
For other languages, the solution uses a triple loop.
However, from the constraints, upper bound is at most 50 x 50 x 50, so it is considered constant.
Another point to be careful is, website visit count should be done on the unique pattern for each user.
Suppose userA's patterns are (a, b, c) and (a, b ,c), and userB's pattern is (a, b, c).
For userA, count (a, b, c) once. For userB, count (a, b, c) as one.
Summing up, the pattern (a, b, c) should be count as 2.
Additionally, if (a, b, a) and (a, b, c) are tie, the lexicographically smaller (a, b, a) is the answer.

The solution consists of three steps.
The first step is to create a user based website list sorted by timestamp.
The hash table is a good data structure to save that.
The second step is to create three websites combinations from the values of the visit history hash table.
While creating the three website combination, count up unique pattern for the individual user.
The last step is to find out the most visited 3 websites combination from the second hash table.
Finally, we will get the answer.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
#include <sstream>
#include <unordered_map>
#include <unordered_set>
#include <vector>

using namespace std;

class AnalyzeUserWebsiteVisitPattern {
public:
    vector<string> mostVisitedPattern(vector<string>& username, vector<int>& timestamp, vector<string>& website) {
        vector<pair<int, int>> t2idx;
        for (int i = 0; i < timestamp.size(); ++i) {
            t2idx.push_back({timestamp[i], i});
        }
        sort(t2idx.begin(), t2idx.end(),
             [](const pair<int, int> &a, const pair<int, int> &b) {
            return a.first < b.first;
        });
        unordered_map<string, vector<string>> visits;
        string u, w, ws;
        for (auto &[t, idx] : t2idx) {
            u = username[idx], w = website[idx];
            visits[u].push_back(w);
        }
        unordered_map<string, int> scores;
        for (auto &[key, value] : visits) {
            if (value.size() < 3) { continue; }
            unordered_set<string> ss;
            for (int i = 0; i < value.size() - 2; ++i) {
                for (int j = i + 1; j < value.size() - 1; ++j) {
                    for (int k = j + 1; k < value.size(); ++k) {
                        ws = value[i] + " " + value[j] + " " + value[k];
                        if (ss.find(ws) == ss.end()) {
                            ss.insert(ws);
                            scores[ws]++;
                        }
                    }
                }
            }
        }
        int max_v = 0;
        ws = "";
        for (auto &[key, value] : scores) {
            if (max_v < value) {
                max_v = value;
                ws = key;
            } else if (max_v == value) {
                ws = min(ws, key);
            }
        }
        vector<string> result(3);
        istringstream iss(ws);
        iss >> result[0] >> result[1] >> result[2];
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
/**
 * @param {string[]} username
 * @param {number[]} timestamp
 * @param {string[]} website
 * @return {string[]}
 */
var mostVisitedPattern = function(username, timestamp, website) {
  const record = timestamp.map((item, i) => [timestamp[i], username[i], website[i]])
    .sort((a, b) => a[0] - b[0]);
  const visits = new Map();
  record.forEach(r => {
    if (!visits.has(r[1])) { visits.set(r[1], new Array()) };
    visits.set(r[1], [...visits.get(r[1]), r[2]]);
  });
  const scores = new Map();
  visits.forEach((value, _) => {
    if (value.length < 3) { return; }
    let ss = new Set();
    for (let i = 0; i < value.length - 2; i++) {
      for (let j = i + 1; j < value.length - 1; j++) {
        for (let k = j + 1; k < value.length; k++) {
          let ws = value[i] + " " + value[j] + " " + value[k];
          if (!ss.has(ws)) {
            ss.add(ws);
            if (!scores.has(ws)) { scores.set(ws, 0); }
            scores.set(ws, scores.get(ws) + 1);
          }
        }
      }
    }
  })
  let max_v = 0, ws = "";
  scores.forEach((value, key) => {
    if (max_v < value) {
      max_v = value;
      ws = key;
    } else if (max_v === value) {
      ws = ws.localeCompare(key) > 0 ? key : ws;
    }
  });
  return ws.split(" ");
};
```
{% endtab %}

{% tab solution Python %}
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
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}


## Complexities
- Time: `O(n * log(n))` -- n: number of website visit record
- Space: `O(m + k)` -- m: number of unique users, k: number of three website patterns

---
layout: post
title: Concatenated Words
algo_menubar: algo_menu
hero_height: is-small
tags:
- Hard
- Dynamic Programming
- Depth-First Search
- String
- Array
date: 2022-09-14 21:51 +0900
---

## Problem Description
> Given an array of strings `words` (without duplicates),
> return all the concatenated words in the given list of words.
>
> A concatenated word is defined as a string that is comprised entirely of
> at least two shorter words in the given array.
>
> Constraints:
> - `1 <= words.length <= 10**4`
> - `1 <= words[i].length <= 30`
> - `words[i]` consists of only lowercase English letters.
> - All the strings of words are unique.
> - `1 <= sum(words[i].length) <= 10**5`
>
> [https://leetcode.com/problems/concatenated-words/](https://leetcode.com/problems/concatenated-words/)

## Examples
```
Example 1:
Input: words = ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]
Output: ["catsdogcats","dogcatsdog","ratcatdogcat"]
```

```
Example 2:
Input: words = ["cat","dog","catdog"]
Output: ["catdog"]
```

## How to Solve
If it is a string problem and a type of substring comparison,
it might be solved by the depth-first search (recursion) changing the length of the word.

An easy approach would be to divide a word to prefix and suffix.
Then, check both prefix and suffix are in the given word list.
Shift the index and divide the word at a new index, then check.

However, as this problem is categorized in Hard, it is very hard problem.
It requires an optimization not to get the time limited exceeded error.

The approach here uses depth-first search (dfs, recursion) and memoization.
Also, the current index is passed to dfs function not to create substrings on memory.
The memoization is used to check whether the index is already visited or not.
During the recursion, only prefix part is looked up to the word set.
The suffix part check will be done in the deeper iteration.
If thr dfs returns truthy, add the word to the result array.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class ConcatenatedWords {
private:
    bool dfs(unordered_set<string> &word_set, vector<int> &memo, string &word, int idx) {
        if (idx == word.size()) { return 1; }
        if (memo[idx] != -1) { return memo[idx]; }
        bool ret = 0;
        for (int i = 1; i <= word.size() - idx - (idx == 0); ++i) {
            if (word_set.find(word.substr(idx, i)) != word_set.end()) {
                ret = dfs(word_set, memo, word, idx + i);
                if (ret) { break; }
            }
        }
        return memo[idx] = ret;
    }

public:
    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
        unordered_set<string> word_set(words.begin(), words.end());
        vector<string> result;
        for (string word : words) {
            vector<int> memo(word.size(), -1);
            if (dfs(word_set, memo, word, 0)) {
                result.push_back(word);
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
/**
 * @param {string[]} words
 * @return {string[]}
 */
var findAllConcatenatedWordsInADict = function(words) {
  let word_set = new Set(words);

  const func = function dfs(memo, word, idx) {
    let n = word.length;
    if (idx === n) { return 1; }
    if (memo[idx] !== -1) { return memo[idx]; }
    let ret = 0;
    for (let i = 1; i <= n - idx - (idx == 0); i++) {
      if (word_set.has(word.substring(idx, idx + i))) {
        ret = dfs(memo, word, idx + i);
        if (ret) { break; }
      }
    }
    memo[idx] = ret;
    return ret;
  }

  let result = new Array();
  for (let word of words) {
    let memo = new Array(word.length).fill(-1);
    if (func(memo, word, 0)) {
      result.push(word);
    }
  }
  return result;
};
```
{% endtab %}

{% tab solution Python %}
```python
class ConcatenatedWords:
    def findAllConcatenatedWordsInADict(self, words: List[str]) -> List[str]:
        word_set = set(words)

        def dfs(memo, word, idx):
            n = len(word)
            if idx == n: return 1
            if memo[idx] != -1: return memo[idx]
            ret = 0
            for i in range(1, n - idx - (idx == 0) + 1):
                if word[idx:idx + i] in word_set:
                    ret = dfs(memo, word, idx + i)
                    if ret: break
            memo[idx] = ret
            return ret

        result = []
        for word in words:
            memo = [-1 for _ in range(len(word))]
            if dfs(memo, word, 0):
                result.append(word)
        return result
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}


## Complexities
- Time: `O(n * m)` --> `O(n)` since m: length of the longest word is at most 30.
- Space: `O(m)` -- for a recursion stack

---
layout: post
title: Concatenated Words
hero_height: is-small
tags:
- Hard
- Depth-First Search
- String
- Dynamic Programming
- Trie
- Array
date: 2022-09-14 21:51 +0900
---
## Introduction
This might be solved by the dynamic programming.
Also, the recursion (depth-first search) with prefix and suffix would work.
In any case, this is hard to solve.

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

## Analysis
Divide each word to prefix and suffix.
The next deeper level checks previous suffix.
In the end, if suffix is in the given word list, the suffix is a part of concatenated word.

## Solution
```python
class ConcatenatedWords:
    def findAllConcatenatedWordsInADict(self, words: List[str]) -> List[str]:
        word_set = set(words)

        def dfs(w):
            n = len(w)
            for i in range(1, n):
                prefix, suffix = w[:i], w[i:]
                if prefix in word_set and (suffix in word_set or dfs(suffix)):
                    return True
            return False
                
        return [word for word in words if dfs(word)]
```

## Complexities
- Time: `O()`
- Space: `O()`

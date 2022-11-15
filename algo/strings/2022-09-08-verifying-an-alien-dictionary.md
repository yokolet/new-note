---
layout: post
title: Verifying an Alien Dictionary
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- String
- Array
- Hash Table
date: 2022-09-08 22:14 +0900
---
## Introduction
The given dictionary letters' order matters.
Hash table is a good data structure to save the letter order.
However, in case of Python, string's index or find method can do the same,
so Python doesn't need Hash table.
Other than that, this problem is a simple string traversal of a pair.

## Problem Description
> In an alien language, surprisingly, they also use English lowercase letters,
> but possibly in a different `order`.
> The order of the alphabet is some permutation of lowercase letters.
>
> Given a sequence of words written in the alien language, and the `order` of the alphabet,
> return true if and only if the given words are sorted lexicographically in this alien language.
>
> Constraints:
> - `1 <= words.length <= 100`
> - `1 <= words[i].length <= 20`
> - `order.length == 26`
> - All characters in `words[i]` and `order` are English lowercase letters.
>
> [https://leetcode.com/problems/verifying-an-alien-dictionary/](https://leetcode.com/problems/verifying-an-alien-dictionary/)

## Examples
```
Example 1:
Input: words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
Output: true
```

```
Example 2:
Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
Output: false
```

```
Example 3:
Input: words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
Output: false
```

## Analysis
Take two words pair one by one.
Find the index of the first mismatch characters.
The previous word's mismatch character index should be smaller if the string is valid.
We should consider one more case.
If one is a substring of another, the previous word's length should be shorter.
If the conditions don't meet, given word list is invalid.

## Solution
```python
class VerifyingAnAlianDictionary:
    def isAlienSorted(self, words: List[str], order: str) -> bool:
        def helper(prev, cur):
            for i in range(min(len(prev), len(cur))):
                if prev[i] != cur[i]:
                    break
                if i == min(len(prev), len(cur)) - 1:
                    return len(cur) - len(prev)
            return order.index(cur[i]) - order.index(prev[i])
        prev = words[0]
        for cur in words[1:]:
            if helper(prev, cur) < 0:
                return False
            prev = cur
        return True
```

## Complexities
- Time: `O(n)` -- n is a total number of characters in words
- Space: `O(1)`

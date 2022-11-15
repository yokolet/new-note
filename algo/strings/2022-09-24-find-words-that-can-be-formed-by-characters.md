---
layout: post
title: Find Words That Can Be Formed by Characters
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Hash Table
- String
date: 2022-09-24 22:56 +0900
---
## Introduction
This is a sort of anagram problem.
A hash table is a good data structure to solve this problem.
Two types of hash tables are necessary: one for the given chars, another for the each word.
If we compare these two hash tables, it's not difficult to find the answer.

## Problem Description
> You are given an array of strings `words` and a string `chars`.
> A string is good if it can be formed by characters from chars (each character can only be used once).
>
> Return the sum of lengths of all good strings in words.
>
> Constraints:
> - `1 <= words.length <= 1000`
> - `1 <= words[i].length, chars.length <= 100`
> - `words[i]` and `chars` consist of lowercase English letters.
>
> [https://leetcode.com/problems/find-words-that-can-be-formed-by-characters/](https://leetcode.com/problems/find-words-that-can-be-formed-by-characters/)

## Examples
```
Example 1
Input: words = ["cat","bt","hat","tree"], chars = "atach"
Output: 6
Explanation: The strings that can be formed are "cat" and "hat" so the answer is 3 + 3 = 6.
```

```
Example 2
Input: words = ["hello","world","leetcode"], chars = "welldonehoneyr"
Output: 10
Explanation: The strings that can be formed are "hello" and "world" so the answer is 5 + 5 = 10.
```

## Analysis
The first step is to create a character frequency table for `chars`.
To check each word in the word list, create another frequency table for the word.
All characters in the word table exist in the first frequency table, also the count is greater than or equal to,
then add the word length to the result.

## Solution
```python
class FindWordsThatCanBeFormedByCharacters:
    def countCharacters(self, words: List[str], chars: str) -> int:
        counter = collections.Counter(chars)

        def isGood(word):
            w_cnt = collections.Counter(word)
            for k, v in w_cnt.items():
                if k not in counter or counter[k] < v:
                    return False
            return True
        
        result = 0
        for word in words:
            if isGood(word):
                result += len(word)
        return result
```

## Complexities
- Time: `O(m + n)` -- m: length of chars, n: number of words
- Space: `O(m + k)` -- k : length of each word

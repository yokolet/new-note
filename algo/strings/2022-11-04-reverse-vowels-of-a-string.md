---
layout: post
title: Reverse Vowels of a String
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- Two Pointers
- String
date: 2022-11-04 14:20 +0900
---
## Introduction
Since the problem asks reversing vowels only, the two pointers approach works well.
Starting from leftmost and rightmost characters, increment or decrement indices until those come to the vowel.
Swap two vowels, then increment the left pointer and decrement the right pointer.
When the left exceeds the right, we can get the answer.

## Problem Description
> Given a string `s`, reverse only all the vowels in the string and return it.
>
> The vowels are 'a', 'e', 'i', 'o', and 'u', and they can appear in both lower and upper cases, more than once.
>
> Constraints:
> - `1 <= s.length <= 3 * 10**5`
> - `s` consist of printable ASCII characters.
>
> [https://leetcode.com/problems/reverse-vowels-of-a-string/](https://leetcode.com/problems/reverse-vowels-of-a-string/)

## Examples
```
Exmaple 1
Input: s = "hello"
Output: "holle"
```

```
Example 2
Input: s = "leetcode"
Output: "leotcede"
```

## Analysis
The solution here took the two pointers approach.
Initialize left and right pointers with leftmost and rightmost indices.
Python doesn't allow assigning the character in a string, so the input string is converted to an array.
Increment the left and decrement the right until those point vowels.
Swap vowel characters and increment/decrement indicies.
When the left exceeds the right, the character array holds the answer.

## Solution
```python
class ReverseVowelsOfAString:
    def reverseVowels(self, s: str) -> str:
        left, right = 0, len(s) - 1
        ss = list(s)
        while left < right:
            while left < len(ss) and ss[left] not in 'aeiouAEIOU':
                left += 1
            while right >= 0 and ss[right] not in 'aeiouAEIOU':
                right -= 1
            if left < right:
                ss[left], ss[right] = ss[right], ss[left]
                left += 1
                right -= 1
        return ''.join(ss)
```

## Complexities
- Time: `O(n)`
- Space: `O(1)`

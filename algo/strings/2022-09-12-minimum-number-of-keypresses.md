---
layout: post
title: Minimum Number of Keypresses
hero_height: is-small
tags:
- Medium
- Sorting
- String
- Greedy
date: 2022-09-12 17:20 +0900
---
## Introduction
String related problems are sometime often confusing.
We should read the problem description carefully.
It might be turned out it is not so difficult like this problem.

## Problem Description
> You have a keypad with 9 buttons, numbered from 1 to 9, each mapped to lowercase English letters.
> You can choose which characters each button is matched to as long as:
> - All 26 lowercase English letters are mapped to.
> - Each character is mapped to by exactly 1 button.
> - Each button maps to at most 3 characters.
>
> To type the first character matched to a button, you press the button once.
> To type the second character, you press the button twice, and so on.
>
> Given a string `s`, return the minimum number of keypresses needed to type s using your keypad.
>
> Note that the characters mapped to by each button, and the order they are mapped in cannot be changed.
>
> Constraints:
> - `1 <= s.length <= 10**5`
> - `s` consists of lowercase English letters.
>
> [https://leetcode.com/problems/minimum-number-of-keypresses/](https://leetcode.com/problems/minimum-number-of-keypresses/)

## Examples
```
Example 1:
Input: s = "apple"
Output: 5
Explanation: One optimal way to setup your keypad is shown above.
Type 'a' by pressing button 1 once.
Type 'p' by pressing button 6 once.
Type 'p' by pressing button 6 once.
Type 'l' by pressing button 5 once.
Type 'e' by pressing button 3 once.
A total of 5 button presses are needed, so return 5.
```

```
Example 2:
Input: s = "abcdefghijkl"
Output: 15
Explanation: One optimal way to setup your keypad is shown above.
The letters 'a' to 'i' can each be typed by pressing a button once.
Type 'j' by pressing button 1 twice.
Type 'k' by pressing button 2 twice.
Type 'l' by pressing button 3 twice.
A total of 15 button presses are needed, so return 15.
```

## Analysis
In this problem, a frequency of characters matters.
The most frequently used characters should be put as the first letter.
The solution starts from counting the characters and sorting based on the frequency.
The number of characters are at most 26.
First 9 frequent characters are all 1.
Then, 10th to 18th characters are 2.
Lastly characters after 18th are 3.
Depends on the input string length, those are counted.

## Solution
```python
class MinimumNumberOfKeypresses:
    def minimumKeypresses(self, s: str) -> int:
        counter = collections.Counter(s)
        counter = sorted(counter.items(), key=lambda x: x[1], reverse=True)
        if len(counter) <= 9:
            return len(s)
        else:
            values = [x[1] for x in counter]
            if len(counter) <= 18:
                return sum(values[:9]) + 2 * sum(values[9:])
            else:
                return sum(values[:9]) + 2 * sum(values[9:18]) + 3 * sum(values[18:])
```

## Complexities
- Time: `O(1)` -- all are up to 26, constant
- Space: `O(1)`

---
layout: post
title: Bag of Tokens
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Sorting
- Two Pointers
- Greedy
date: 2022-09-12 13:08 +0900
---
## Introduction
Since the order of the given array doesn't matter, start from sorting the array.
The greedy approach can be applied.

## Problem Description
> You have an initial power of `power`, an initial score of `0`, and
> a bag of `tokens` where `tokens[i]` is the value of the i-th token (0-indexed).
> Your goal is to maximize your total score by potentially playing each token in one of two ways:
> - If your current `power` is at least `tokens[i]`, you may play the i-th token face up,
>   losing `tokens[i]` power and gaining `1` score.
> - If your current `score` is at least 1, you may play the i-th token face down,
>   gaining `tokens[i]` power and losing `1` score.
>   
> Each token may be played at most once and in any order. You do not have to play all the tokens.
>
>Return the largest possible score you can achieve after playing any number of tokens.
>
> Constraints:
> - `0 <= tokens.length <= 1000`
> - `0 <= tokens[i], power < 10**4`
>
> [https://leetcode.com/problems/bag-of-tokens/](https://leetcode.com/problems/bag-of-tokens/)

## Examples
```
Example 1:
Input: tokens = [100], power = 50
Output: 0
Explanation: Playing the only token in the bag is impossible because you either have too little power or
too little score.
```

```
Example 2:
Input: tokens = [100,200], power = 150
Output: 1
Explanation: Play the 0th token (100) face up, your power becomes 50 and score becomes 1.
There is no need to play the 1st token since you cannot play it face up to add to your score.
```

```
Example 3:
Input: tokens = [100,200,300,400], power = 200
Output: 2
Explanation: Play the tokens in this order to get a score of 2:
1. Play the 0th token (100) face up, your power becomes 100 and score becomes 1.
2. Play the 3rd token (400) face down, your power becomes 500 and score becomes 0.
3. Play the 1st token (200) face up, your power becomes 300 and score becomes 1.
4. Play the 2nd token (300) face up, your power becomes 0 and score becomes 2.
```

## Analysis
Since the order doesn't matter, start from sorting the token array.
Take smaller token to get a score, then take bigger token to gain power.
The process lose opposites, power for more scores, scores for more power.
Use two pointers, low and high.
As the low pointer proceeds, it gains the score and loses power.
When the power becomes lower than the current token, the high pointer starts working.
When the score becomes 0, it's back to low pointer to get more scores.

## Solution
```python
class BagOfTokens:
    def bagOfTokensScore(self, tokens: List[int], power: int) -> int:
        tokens.sort()
        score = 0
        low, high = 0, len(tokens) - 1
        while low <= high:
            if power >= tokens[low]:
                power -= tokens[low]
                score += 1
                low += 1
            elif low < high and score:
                power += tokens[high]
                score -= 1
                high -= 1
            else:
                break
        return score
```

## Complexities
- Time: `O(nlog(n))`
- Space: `O(1)`

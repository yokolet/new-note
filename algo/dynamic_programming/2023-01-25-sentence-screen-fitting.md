---
layout: post
title: Sentence Screen Fitting
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Dynamic Programming
- String
date: 2023-01-25 15:43 +0900
---
## Problem Description
> Given a `rows x cols` screen and a `sentence` represented as a list of strings, return the number of times the given
> sentence can be fitted on the screen.
>
> The order of words in the sentence must remain unchanged, and a word cannot be split into two lines. A single space
> must separate two consecutive words in a line.
>
> Constraints:
> `-1 <= sentence.length <= 100`
> - `1 <= sentence[i].length <= 10`
> - `sentence[i]` consists of lowercase English letters.
> - `1 <= rows, cols <= 2 * 10**4`
>
> [https://leetcode.com/problems/sentence-screen-fitting/](https://leetcode.com/problems/sentence-screen-fitting/)

## Examples
```
Example 1
Input: sentence = ["hello","world"], rows = 2, cols = 8
Output: 1
Explanation:
hello___
world___
```

```
Example 2
Input: sentence = ["a", "bcd", "e"], rows = 3, cols = 6
Output: 2
Explanation:
a_bcd_
e_a___
bcd_e_
```

```
Example 3
Input: sentence = ["i","had","apple","pie"], rows = 4, cols = 5
Output: 1
Explanation:
i_had
apple
pie_i
had__
```

## How to Solve
The memoization (dynamic programming) is a good approach for this problem.
The auxiliary array saves the each character position and word end marker of -1.
Once auxiliary array is created, count how many entire sentence fit in a single row.
The count goes up to the end of rows * cols, so the answer should be divided by the sentence length.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class SentenceScreenFitting {
public:
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
        string words = "";
        for (string &word : sentence) {
            words += word + ' ';
        }
        int start = -1, n = words.size();
        vector<int> memo(n, 0);
        for (int i = 0; i < n; ++i) {
            if (words[i] == ' ') {
                start = i;
            }
            memo[i] = i - start - 1;
        }
        int length = 0;
        for (int r = 0; r < rows; ++r) {
            length += cols;
            length -= memo[length % n];
        }
        return (length + 1) / n;
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

```
{% endtab %}

{% tab solution Python %}
```python
class SentenceScreenFitting:
    def wordsTyping(self, sentence: List[str], rows: int, cols: int) -> int:
        words = ' '.join(sentence) + ' '
        start, n = -1, len(words)
        memo = [0 for _ in range(n)]
        for i in range(n):
            if words[i] == ' ':
                start = i
            memo[i] = i - start - 1
        length = 0
        for _ in range(rows):
            length += cols
            length -= memo[length % n]
        return (length + 1) // n
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(n)` -- n: number of rows. A sentence and each word have upper bound of 100 and 10 respectively. Those are constant
- Space: `O(1)`

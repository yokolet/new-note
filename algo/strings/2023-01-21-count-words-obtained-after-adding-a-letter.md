---
layout: post
title: Count Words Obtained After Adding a Letter
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Sorting
- String
date: 2023-01-21 23:51 +0900
---
## Problem Description
> You are given two 0-indexed arrays of strings `startWords` and `targetWords`. Each string consists of lowercase
> English letters only.
> For each string in `targetWords`, check if it is possible to choose a string from `startWords` and perform a
> conversion operation on it to be equal to that from `targetWords`.
> The conversion operation is described in the following two steps:
> - Append any lowercase letter that is not present in the string to its end.
>   - For example, if the string is "abc", the letters 'd', 'e', or 'y' can be added to it, but not 'a'. If 'd' is
>     added, the resulting string will be "abcd".
> - Rearrange the letters of the new string in any arbitrary order.
>   - For example, "abcd" can be rearranged to "acbd", "bacd", "cbda", and so on. Note that it can also be rearranged
>     to "abcd" itself.
>
> Return the number of strings in `targetWords` that can be obtained by performing the operations on any string of
> `startWords`.
>
> Note that you will only be verifying if the string in `targetWords` can be obtained from a string in startWords by
> performing the operations. The strings in `startWords` do not actually change during this process.
>
> Constraints:
> - `1 <= startWords.length, targetWords.length <= 5 * 10**4`
> - `1 <= startWords[i].length, targetWords[j].length <= 26`
> - Each string of `startWords` and `targetWords` consists of lowercase English letters only.
> - No letter occurs more than once in any string of `startWords` or `targetWords`.
>
> [https://leetcode.com/problems/count-words-obtained-after-adding-a-letter/](https://leetcode.com/problems/count-words-obtained-after-adding-a-letter/)

## Examples
```
Exmaple 1
Input: startWords = ["ant","act","tack"], targetWords = ["tack","act","acti"]
Output: 2
Explanation:
act -> tack and act -> acti
```

```
Example 2
Input: startWords = ["ab","a"], targetWords = ["abc","abcd"]
Output: 1
Explanation:
ab -> abc
```

## How to Solve
This problem is an anagram comparison.
Both start and target words should be sorted for comparison.
The solution here took a string matching approach by creating a one letter shorter word from targets.
If the matching word is found in start words, count up.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class Solution {
public:
    int wordCount(vector<string>& startWords, vector<string>& targetWords) {
        unordered_set<string> starts;
        for (auto &word : startWords) {
            sort(word.begin(), word.end());
            starts.insert(word);
        }
        int result = 0;
        for (auto &word : targetWords) {
            sort(word.begin(), word.end());
            for (int i = 0; i < word.size(); ++i) {
                if (starts.find(word.substr(0, i) + word.substr(i + 1)) != starts.end()) {
                    result++;
                    break;
                }
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

```
{% endtab %}

{% tab solution Python %}
```python
class Solution:
    def wordCount(self, startWords: List[str], targetWords: List[str]) -> int:
        starts = set()
        for word in startWords:
            word = "".join(sorted(word))
            starts.add(word)
        result = 0
        for word in targetWords:
            word = "".join(sorted(word))
            for i in range(len(word)):
                if word[:i] + word[i + 1:] in starts:
                    result += 1
                    break
        return result
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(m + n)` -- m, n: number of start and target words. Sorting is constant since each word has at most 26 characters.
- Space: `O(m)`

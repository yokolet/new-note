---
layout: post
title: License Key Formatting
algo_menubar: algo_menu
hero_height: is-small
tags:
- Easy
- String
date: 2022-12-09 14:11 +0900
---
## Problem Description
> You are given a license key represented as a string `s` that consists of only alphanumeric characters and dashes.
> The string is separated into `n + 1` groups by `n` dashes. You are also given an integer `k`.
>
> We want to reformat the string s such that each group contains exactly k characters, except for the first group,
> which could be shorter than `k` but still must contain at least one character. Furthermore, there must be a dash
> inserted between two groups, and you should convert all lowercase letters to uppercase.
>
> Return the reformatted license key.
>
> Constraints:
> - `1 <= s.length <= 10**5`
> - `s` consists of English letters, digits, and dashes '-'.
> - `1 <= k <= 10**4`
>
> [https://leetcode.com/problems/license-key-formatting/](https://leetcode.com/problems/license-key-formatting/)

## Examples
```
Example 1
Input: s = "5F3Z-2e-9-w", k = 4
Output: "5F3Z-2E9W"
Explanation: The string s has been split into two parts, each part has 4 characters.
Note that the two extra dashes are not needed and can be removed.
```

```
Example 2
Input: s = "2-5g-3-J", k = 2
Output: "2-5G-3J"
Explanation: The string s has been split into three parts, each part has 2 characters except the first part as it could
be shorter as mentioned above.
```

## How to Solve
The dashes in the input string should be removed. Then, the string is checked from last to first.
In this solution, the given string is reversed first to make reverse traversal easy.
Go over the reversed string character by character while counting the number of characters.
Skip "-" and add upper case characters to a result string.
When the count becomes k, add "-" and initialize the count.
Once all characters are checked, reverse the result string to make it back.
The first character may be "-", so check it and return a substring or entire string.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
class LicenseKeyFormatting {
public:
    string licenseKeyFormatting(string s, int k) {
        reverse(s.begin(), s.end());
        string result = "";
        int count = 0;
        for (auto c : s) {
            if (c != '-') {
                result += toupper(c);
                count++;
            }
            if (count == k) {
                result += '-';
                count = 0;
            }
        }
        reverse(result.begin(), result.end());
        return result.front() == '-' ? result.substr(1) : result;
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
class LicenseKeyFormatting:
    def licenseKeyFormatting(self, s: str, k: int) -> str:
        s = s[::-1]
        result, count = '', 0
        for c in s:
            if c != '-':
                result += c.upper()
                count += 1
            if count == k:
                result += '-'
                count = 0
        result = result[::-1]
        return result[1:] if result and result[0] == '-' else result
```
{% endtab %}

{% tab solution Ruby %}
```ruby

```
{% endtab %}

{% endtabs %}



## Complexities
- Time: `O(n)`
- Space: `O(1)`

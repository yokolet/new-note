---
layout: post
title: Maximum Number of Books You Can Take
algo_menubar: algo_menu
hero_height: is-small
tags:
- Hard
- Dynamic Programming
- Monotonic Stack
date: 2022-09-19 22:50 +0900
---
## Introduction
If we think of the height of books, it's easy to think of the monotonic stack.
However, that's not the only one algorithm here.
It needs dynamic programming approach as well to keep tracking the maximum value at index i.
By comparison of the maximum value so far, we can get the answer.

## Problem Description
> You are given a 0-indexed integer array `books` of length `n`
> where `books[i]` denotes the number of books on the i-th shelf of a bookshelf.
>
> You are going to take books from a contiguous section of the bookshelf spanning
> from `l` to `r` where `0 <= l <= r < n`.
> For each index `i` in the range `l <= i < r`, you must take strictly fewer books
> from shelf `i` than shelf `i + 1`.
>
> Return the maximum number of books you can take from the bookshelf.
>
> Constraints:
> - `1 <= books.length <= 10**5`
> - `0 <= books[i] <= 10**5`
>
> [https://leetcode.com/problems/maximum-number-of-books-you-can-take/](https://leetcode.com/problems/maximum-number-of-books-you-can-take/)

## Examples
```
Example 1
Input: books = [8,5,2,7,9]
Output: 19
Explanation:
- Take 1 book from shelf 1.
- Take 2 books from shelf 2.
- Take 7 books from shelf 3.
- Take 9 books from shelf 4.
You have taken 19 books, so return 19.
It can be proven that 19 is the maximum number of books you can take.
```

```
Example 2
Input: books = [7,0,3,4,5]
Output: 12
Explanation:
- Take 3 books from shelf 2.
- Take 4 books from shelf 3.
- Take 5 books from shelf 4.
You have taken 12 books so return 12.
It can be proven that 12 is the maximum number of books you can take.
```

```
Example 3
Input: books = [8,2,3,7,3,4,0,1,4,3]
Output: 13
Explanation:
- Take 1 book from shelf 0.
- Take 2 books from shelf 1.
- Take 3 books from shelf 2.
- Take 7 books from shelf 3.
You have taken 13 books so return 13.
It can be proven that 13 is the maximum number of books you can take.
```

## Analysis
This is monotonically increasing stack problem.
To count the number of books, the solution uses a simple formula:
the sum of 1, 2, 3, ..., n is n * (n + 1) / 2.
The range of books starts from 0 or the index of previously smallest stack.
While updating the maximum value at index i, update the ground maximum value as well.

## Solution
```python
class MaximumNumberOfBooksYouCanTake:
    def maximumBooks(self, books: List[int]) -> int:
        n, stack = len(books), []
        memo = [0 for _ in range(n)]
        result = 0
        for i in range(n):
            while stack and books[stack[-1]] >= books[i] - (i - stack[-1]):
                stack.pop()
            cur = books[i] * (books[i] + 1) // 2
            if not stack:
                idx = max(0, books[i] - (i + 1))
                cur -= idx * (idx + 1) // 2
            else:
                idx = books[i] - (i - stack[-1])
                cur -= idx * (idx + 1) // 2
                cur += memo[stack[-1]]
            result = max(result, cur)
            memo[i] = cur
            stack.append(i)
        return result
```

## Complexities
- Time: `O(n)`
- Space: `O(n)`

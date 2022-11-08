---
layout: post
title: Word Search
hero_height: is-small
tags:
- Medium
- Backtracking
- Matrix
- Array
date: 2022-09-23 17:11 +0900
---
## Introduction
This is a typical backtracking problem. Normally, a depth-first search works well.
For the backtracking problem, how to get the state back to previous one is important.
The solution here uses a special character as visited status and set it back when the process goes back.

## Problem Description
> Given an `m x n` grid of characters `board` and a string `word`, return true if word exists in the grid.
>
> The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are
> horizontally or vertically neighboring. The same letter cell may not be used more than once.
>
> Constraints:
> - `m == board.length`
> - `n = board[i].length`
> - `1 <= m, n <= 6`
> - `1 <= word.length <= 15`
> - `board` and `word` consists of only lowercase and uppercase English letters.
>
> [https://leetcode.com/problems/word-search/](https://leetcode.com/problems/word-search/)

## Examples
```
Example 1
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
Explanation:
[A] [B] [C]  E
 S   F  [C]  S
 A  [D] [E]  E
```

```
Example 2
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
Explanation:
 A   B   C   E
 S   F   C  [S]
 A   D  [E] [E]
```

```
Example 3
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```

## Analysis
Since it is a backtracking problem, the depth-first approach is used here.
As a visited state management, a special character '#' is used. 
Save the current character on the board to make it back later.
Set the special character to the cell and go deeper recursion with the suffix of the given word.
When the suffix becomes empty, all character in the give word is found.

## Solution
```python
class WordSearch:
    def exist(self, board: List[List[str]], word: str) -> bool:
        m, n = len(board), len(board[0])

        def dfs(row, col, suffix):
            if not suffix:
                return True
            cur = board[row][col]
            board[row][col] = '#'
            up = row - 1 >= 0 and board[row - 1][col] == suffix[0] and dfs(row - 1, col, suffix[1:])
            left = col - 1 >= 0 and board[row][col - 1] == suffix[0] and dfs(row, col - 1, suffix[1:])
            right = col + 1 < n and board[row][col + 1] == suffix[0] and dfs(row, col + 1, suffix[1:])
            down = row + 1 < m and board[row + 1][col] == suffix[0] and dfs(row + 1, col, suffix[1:])
            if up or left or right or down:
                return True
            board[row][col] = cur

        for row in range(m):
            for col in range(n):
                if board[row][col] == word[0] and dfs(row, col, word[1:]):
                    return True
        return False
```

## Complexities
- Time: `O(n * 3 ^ k)` -- n: number of cells, k: length of the word, 3: one of four directions is where it comes from
- Space: `O(k)`

---
layout: post
title: Design Snake Game
hero_height: is-small
tags:
- Medium
- Design
- Queue
- Matrix
date: 2022-10-31 15:33 +0900
---
## Introduction
This problem is a well-known snake game implementation.
How to manage the snake body coordinates is the key to solve this problem.
When the snake head collides in to its body, the function should return -1.
When the snake head comes to the food location, its body gets longer by one.
Given those requirements, the snake body should be an array.
When the snake gets longer, simply add its head to the array.
When the snake moves without getting longer, remove the first coordinate from the array.
Other than that, when the snake head goes out of the grid, it returns -1.
When the snake head is on the food location, increment the score.

## Problem Description
> Design a Snake game that is played on a device with screen size `height x width`. Play the game online if you are
> not familiar with the game.
>
> The snake is initially positioned at the top left corner `(0, 0)` with a length of `1` unit.
> You are given an array `food` where `food[i] = (r[i], c[i])` is the row and column position of a piece of food that
> the snake can eat. When a snake eats a piece of food, its length and the game's score both increase by 1.
> Each piece of food appears one by one on the screen, meaning the second piece of food will not appear until the
> snake eats the first piece of food. When a piece of food appears on the screen, it is guaranteed that it will not
> appear on a block occupied by the snake.
>
> The game is over if the snake goes out of bounds (hits a wall) or if its head occupies a space that its body occupies
> after moving (i.e. a snake of length 4 cannot run into itself).
>
> Implement the SnakeGame class:
> - `SnakeGame(int width, int height, int[][] food)` Initializes the object with a screen of size height x width and
>    the positions of the food.
> - `int move(String direction)` Returns the score of the game after applying one direction move by the snake. If the
>    game is over, return -1.
>
> Constraints:
> - `1 <= width, height <= 10**4`
> - `1 <= food.length <= 50`
> - `food[i].length == 2`
> - `0 <= r[i] < height`
> - `0 <= c[i] < width`
> - `direction.length == 1`
> - direction is 'U', 'D', 'L', or 'R'.
> - At most 10**4 calls will be made to `move`.
>
> []()

## Examples
```
Example 1
Input
["SnakeGame", "move", "move", "move", "move", "move", "move"]
[[3, 2, [[1, 2], [0, 1]]], ["R"], ["D"], ["R"], ["U"], ["L"], ["U"]]
Output
[null, 0, 0, 1, 1, 2, -1]

Explanation
SnakeGame snakeGame = new SnakeGame(3, 2, [[1, 2], [0, 1]]);
snakeGame.move("R"); // return 0
snakeGame.move("D"); // return 0
snakeGame.move("R"); // return 1, snake eats the first piece of food. The second piece of food appears at (0, 1).
snakeGame.move("U"); // return 1
snakeGame.move("L"); // return 2, snake eats the second food. No more food appears.
snakeGame.move("U"); // return -1, game over because snake collides with border
```

## Analysis
The snake should save all coordinates it passed, so it is an array of (row, col) tuple.
Calculate the snake's new head, and check it is within the grid.
If not return -1.
Also, check the first food location.
If the snake head is on the first food location, remove the first food from the array and increment the score.
If it is not on the food location, the snake body won't get longer.
Remove the first coordinate from the snake array, which is the equivalent to just move in the same length.
Lastly, add the new snake head to to array.

## Solution
```python
class SnakeGame:

    def __init__(self, width: int, height: int, food: List[List[int]]):
        self.rows = height
        self.cols = width
        self.food = food
        self.snake = [(0, 0)]  # (row, col)
        self.score = 0
        

    def move(self, direction: str) -> int:
        dirs = {
            'U': (-1, 0),
            'D': (1, 0),
            'L': (0, -1),
            'R': (0, 1)
        }
        sr, sc = self.snake[-1][0] + dirs[direction][0], self.snake[-1][1] + dirs[direction][1]
        if sr < 0 or sr >= self.rows or sc < 0 or sc >= self.cols:
            return -1
        if self.food and sr == self.food[0][0] and sc == self.food[0][1]:
            self.food.pop(0)
            self.score += 1
        else:
            self.snake.pop(0)
        if (sr, sc) in self.snake:
            return -1
        self.snake.append((sr, sc))
        return self.score
```

## Complexities
- Time: `O(1)`
- Space: `O(m * n + k)` -- m, n: grid's width, height, k: number of food

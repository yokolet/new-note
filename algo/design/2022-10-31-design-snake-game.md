---
layout: post
title: Design Snake Game
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Design
- Queue
- Matrix
date: 2022-10-31 15:33 +0900
---

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
> [https://leetcode.com/problems/design-snake-game/](https://leetcode.com/problems/design-snake-game/)

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

## How to Solve
This problem is a well-known snake game implementation.
How to manage the snake body coordinates is the key to solve this problem.
When the snake head collides in to its body, the function returns -1.
When the snake head goes out of the grid, the function returns -1.
When the snake head comes to the food location, its body gets longer by one.
Also, the score will be incremented.
Given those requirements, the snake body should be a array-like which has a snake head on the last and tail on the top.
To move the snake, add a head and delete a tail.
To make the snake longer, just add the head and don't delete the tail.

The snake body collision check depends on the language.
Python is easy since its array has ability to find an element.
Other languages need an additional data structure such as set.

When the move method gives direction, calculate the next head position.
After the validity check, if the snake head is on a food location, increment the score.
Otherwise, delete snake's tail to make it looks like moved.
Lastly, add the snake head to array and/or set.

## Solution

{% tabs solution is-boxed %}

{% tab solution C++ %}
```cpp
#include <deque>
#include <set>
#include <unordered_map>
#include <vector>

using namespace std;

class SnakeGame {
private:
    int width_, height_;
    vector<vector<int>> food_;
    int f_idx = 0;
    deque<pair<int, int>> snake;  // {row, col}
    set<int> inuse;
    unordered_map<string, pair<int, int>> dirs = {
        {"U", {-1, 0}},
        {"D", {1, 0}},
        {"L", {0, -1}},
        {"R", {0, 1}}
    };
    int score = 0;
    int rc2idx(int row, int col) {
        return row * width_ + col;
    }

public:
    SnakeGame(int width, int height, vector<vector<int>>& food) :
        width_(width), height_(height), food_(food) {
        snake.push_back(make_pair(0, 0));
        inuse.insert(rc2idx(0, 0));
    }

    int move(string direction) {
        int sr = snake.back().first + dirs[direction].first;
        int sc = snake.back().second + dirs[direction].second;
        if (sr < 0 || sr >= height_ || sc < 0 || sc >= width_) { return -1; }
        if (f_idx < food_.size() && sr == food_[f_idx][0] && sc == food_[f_idx][1]) {
            f_idx++;
            score++;
        } else {
            pair<int, int> tail = snake.front();
            snake.pop_front();
            inuse.erase(rc2idx(tail.first, tail.second));
        }
        int head = rc2idx(sr, sc);
        if (inuse.find(head) != inuse.end()) {
            return -1;
        }
        snake.push_back(make_pair(sr, sc));
        inuse.insert(head);
        return score;
    }
};
```
{% endtab %}

{% tab solution Java %}
```java
import java.util.*;

public class SnakeGame {
    private int width_, height_;
    int[][] food_;
    private int f_idx = 0;
    private Deque<List<Integer>> snake = new ArrayDeque<>();
    Set<Integer> inuse = new HashSet<>();
    private final static Map<String, List<Integer>> dirs = Map.of(
            "U", Arrays.asList(-1, 0),
            "D", Arrays.asList(1, 0),
            "L", Arrays.asList(0, -1),
            "R", Arrays.asList(0, 1)
    );
    int score = 0;
    int rc2idx(int row, int col) {
        return row * width_ + col;
    }

    public SnakeGame(int width, int height, int[][] food) {
        width_ = width;
        height_ = height;
        food_ = food;
        snake.add(Arrays.asList(0, 0));
        inuse.add(0);
    }

    public int move(String direction) {
        List<Integer> head = snake.peekLast();
        int sr = head.get(0) + dirs.get(direction).get(0);
        int sc = head.get(1) + dirs.get(direction).get(1);
        if (sr < 0 || sr >= height_ || sc < 0 || sc >= width_) { return -1; }
        if (f_idx < food_.length && sr == food_[f_idx][0] && sc == food_[f_idx][1]) {
            f_idx++;
            score++;
        } else {
            List<Integer> tail = snake.pollFirst();
            inuse.remove(rc2idx(tail.get(0), tail.get(1)));
        }
        int newHead = rc2idx(sr, sc);
        if (inuse.contains(newHead)) { return -1; }
        snake.add(Arrays.asList(sr, sc));
        inuse.add(newHead);
        return score;
    }
}
```
{% endtab %}

{% tab solution JavaScript %}
```js
/**
 * @param {number} width
 * @param {number} height
 * @param {number[][]} food
 */
var SnakeGame = function(width, height, food) {
  this.width_ = width;
  this.height_ = height;
  this.food_ = food;
  this.snake = [[0, 0]];
  this.inuse = new Set([0]);
  this.dirs = {
    "U": [-1, 0],
    "D": [1, 0],
    "L": [0, -1],
    "R": [0, 1]
  };
  this.score = 0;
  this.rc2idx = (row, col) => {
    return row * this.width_ + col;
  };
};

/**
 * @param {string} direction
 * @return {number}
 */
SnakeGame.prototype.move = function(direction) {
  let sr = this.snake[this.snake.length - 1][0] + this.dirs[direction][0];
  let sc = this.snake[this.snake.length - 1][1] + this.dirs[direction][1];
  if (sr < 0 || sr >= this.height_ || sc < 0 || sc >= this.width_) { return -1; }
  if (this.food_.length > 0 && sr == this.food_[0][0] && sc == this.food_[0][1]) {
    this.food_.shift();
    this.score += 1;
  } else {
    let tail = this.snake.shift();
    this.inuse.delete(this.rc2idx(tail[0], tail[1]));
  }
  let head = this.rc2idx(sr, sc);
  if (this.inuse.has(head)) { return -1; }
  this.snake.push([sr, sc]);
  this.inuse.add(head);
  return this.score;
};
```
{% endtab %}

{% tab solution Python %}
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
{% endtab %}

{% tab solution Ruby %}
```ruby
class SnakeGame

=begin
    :type width: Integer
    :type height: Integer
    :type food: Integer[][]
=end
  def initialize(width, height, food)
    @width_ = width
    @height_ = height
    @food_ = food
    @snake = [[0, 0]]
    @dirs = {
      "U" => [-1, 0],
      "D" => [1, 0],
      "L" => [0, -1],
      "R" => [0, 1]
    }
    @score = 0
  end


=begin
    :type direction: String
    :rtype: Integer
=end
  def move(direction)
    sr, sc = @snake[-1][0] + @dirs[direction][0], @snake[-1][1] + @dirs[direction][1]
    if sr < 0 || sr >= @height_ || sc < 0 || sc >= @width_
      return -1
    end
    if !@food_.empty?  && [sr, sc] == @food_[0]
      @food_.shift
      @score += 1
    else
      @snake.shift
    end
    if @snake.include?([sr, sc])
      return -1
    end
    @snake << [sr, sc]
    @score
  end


end
```
{% endtab %}

{% endtabs %}


## Complexities
- Time: `O(1)`
- Space: `O(m * n + k)` -- m, n: grid's width, height, k: number of food

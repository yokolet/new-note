---
layout: post
title: My Calendar I
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Binary Search
- Design
date: 2022-10-07 17:48 +0900
---
## Introduction
This problem is one of my calendar series and easier one.
The start and end time pair can be saved in a single array.
In such a case, the array will have values: `[start[0], end[0], start[1], end[1], ...]`.
The start times are always on the even indices, while the end times are on odd indices.
Using Python's bisect search, the insertion point can be found.
If the start time insertion point is not on even index, the range overlaps to some of existing events.
This way, we can check a double booking.


## Problem Description
> You are implementing a program to use as your calendar. We can add a new event if adding the event will not
> cause a double booking.
> A double booking happens when two events have some non-empty intersection (i.e., some moment is common to both events.).
>
> The event can be represented as a pair of integers `start` and `end` that represents a booking on
> the half-open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.
>
> Implement the MyCalendar class:
> - `MyCalendar()` Initializes the calendar object.
> - `boolean book(int start, int end)` Returns `true` if the event can be added to the calendar
>    successfully without causing a double booking. Otherwise, return `false` and do not add the event to the calendar.
>
> Constraints:
> - `0 <= start < end <= 10**9`
> - At most 1000 calls will be made to book.
>
> [https://leetcode.com/problems/my-calendar-i/](https://leetcode.com/problems/my-calendar-i/)

## Examples
```
Example 1
Input
["MyCalendar", "book", "book", "book"]
[[], [10, 20], [15, 25], [20, 30]]
Output
[null, true, false, true]

Explanation
MyCalendar myCalendar = new MyCalendar();
myCalendar.book(10, 20); // return True
myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event.
myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20,
                            but not including 20.
```

## Analysis
The solution here uses a single array, which has the values like, `[start[0], end[0], start[1], end[1], ...]`.
The even indices are start times, while odd indices are end times.
When the book method is called, it finds the insertion point by Python's bisect search.
If the index for start is not even, the range overlaps.
If the index for end is not the same as start, the range overlaps.
If it is not a double booking, insert the start and end at the insertion point.

## Solution
```python
class MyCalendar:

    def __init__(self):
        self.bookings = []
        

    def book(self, start: int, end: int) -> bool:
        if end <= start:
            return False
        left = bisect.bisect_right(self.bookings, start)
        if left % 2 == 1:
            return False
        right = bisect.bisect_left(self.bookings, end)
        if left != right:
            return False
        self.bookings[left:left] = [start, end]
        return True
```

## Complexities
- Time: `O(n + log(n))`
- Space: `O(n)`

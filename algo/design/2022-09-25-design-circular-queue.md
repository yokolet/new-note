---
layout: post
title: Design Circular Queue
algo_menubar: algo_menu
hero_height: is-small
tags:
- Medium
- Queue
- Design
- Array
date: 2022-09-25 21:18 +0900
---
## Introduction
Basically, an array and pointer is all to implement the circular queue.
It needs much consideration when the enqueue index comes back to the top.
An additional parameter, size -- current size of the queue, will help to implement easily.

## Problem Description
> Design your implementation of the circular queue.
> The circular queue is a linear data structure in which the operations are performed based on
> FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle.
> It is also called "Ring Buffer".
>
> One of the benefits of the circular queue is that we can make use of the spaces in front of the queue.
> In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue.
> But using the circular queue, we can use the space to store new values.
>
> Implementation the MyCircularQueue class:
> - `MyCircularQueue(k)` Initializes the object with the size of the queue to be `k`.
> - `int Front()` Gets the front item from the queue. If the queue is empty, return -1.
> - `int Rear()` Gets the last item from the queue. If the queue is empty, return -1.
> - `boolean enQueue(int value)` Inserts an element into the circular queue. Return true if the operation is successful.
> - `boolean deQueue()` Deletes an element from the circular queue. Return true if the operation is successful.
> - `boolean isEmpty()` Checks whether the circular queue is empty or not.
> - `boolean isFull()` Checks whether the circular queue is full or not.
>
> You must solve the problem without using the built-in queue data structure in your programming language.
>
> Constraints:
> - `1 <= k <= 1000`
> - `0 <= value <= 1000`
> - At most 3000 calls will be made to `enQueue`, `deQueue`, `Front`, `Rear`, `isEmpty`, and `isFull`.
>
> [https://leetcode.com/problems/design-circular-queue/](https://leetcode.com/problems/design-circular-queue/)

## Examples
```
Example 1

Input
["MyCircularQueue", "enQueue", "enQueue", "enQueue", "enQueue", "Rear", "isFull", "deQueue", "enQueue", "Rear"]
[[3], [1], [2], [3], [4], [], [], [], [4], []]
Output
[null, true, true, true, false, 3, true, true, true, 4]

Explanation
MyCircularQueue myCircularQueue = new MyCircularQueue(3);
myCircularQueue.enQueue(1); // return True
myCircularQueue.enQueue(2); // return True
myCircularQueue.enQueue(3); // return True
myCircularQueue.enQueue(4); // return False
myCircularQueue.Rear();     // return 3
myCircularQueue.isFull();   // return True
myCircularQueue.deQueue();  // return True
myCircularQueue.enQueue(4); // return True
myCircularQueue.Rear();     // return 4
```

```
Example 2
Input
["MyCircularQueue","enQueue","Rear","Front","deQueue","Front","deQueue","Front","enQueue","enQueue","enQueue","enQueue"]
[[3],[2],[],[],[],[],[],[],[4],[2],[2],[3]]
Output
[null,true,2,2,true,-1,false,-1,true,true,true,false]
```

## Analysis
As instance variables, the class has capacity k, queue array, head pointer and queue size.
When the size is zero, the queue is empty.
While the size is the same as the capacity k, the queue is full.
Only when the queue is not full, a new value can be added.
The index is the current head plus size modulo capacity.
Only when the queue is not empty, the deQueue operation is possible.
Move the head to the current head plus one module capacity.

## Solution
```python
class MyCircularQueue:

    def __init__(self, k: int):
        self.k = k
        self.queue = [-1 for _ in range(k)]
        self.head = 0
        self.size = 0

    def enQueue(self, value: int) -> bool:
        if self.isFull():
            return False
        idx = (self.head + self.size) % self.k
        self.queue[idx] = value
        self.size += 1
        return True

    def deQueue(self) -> bool:
        if self.isEmpty():
            return False
        self.head = (self.head + 1) % self.k
        self.size -= 1
        return True

    def Front(self) -> int:
        if self.isEmpty():
            return -1
        return self.queue[self.head]

    def Rear(self) -> int:
        if self.isEmpty():
            return -1
        idx = (self.head + self.size - 1) % self.k
        return self.queue[idx]

    def isEmpty(self) -> bool:
        return self.size == 0

    def isFull(self) -> bool:
        return self.size == self.k
```

## Complexities
- Time: `O(1)`
- Space: `O(k)`
 

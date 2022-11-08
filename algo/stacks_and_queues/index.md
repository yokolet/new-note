---
layout: page
title: Algorithm
subtitle: Stacks and Queues
menubar: stacks_and_queues_menu
hero_image: /assets/img/fall_pond.jpeg
show_sidebar: false
---

## Algorithm
### Stacks and Queues
This section has problems which can be solved by stacks or queues.

To figure out it is a stack problem, the idea of **monotonic stack** is important.
The monotonic stack is, values in the stack are monotonically increasing or decreasing.

For example, the values in the stack is: `[6, 4, 2]`. It is monotonically decreasing.
When a new value, `5` comes in, `2` is popped out, then `4` is popped out also.
The updated stack will be: `[6, 5]`, which is monotonically decreasing with the new value `5`.

Another point is what should be saved in the stack.
It might be a value (or character), but in another case, it might be an index.
If the problem needs the monotonic stack, often, the values are indices.

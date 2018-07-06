---
description: 只是结合了具体的应用场景，本质上还是比较简单的BFS，跳过吧。。
---

# Knight Shortest Path

## Description

Given a knight in a chessboard \(a binary matrix with `0` as empty and `1` as barrier\) with a `source` position, find the shortest path to a `destination` position, return the length of the route.  
Return `-1` if knight can not reached.

## Clarification

If the knight is at \(_x_, _y_\), he can get to the following positions in one step:

```text
(x + 1, y + 2)
(x + 1, y - 2)
(x - 1, y + 2)
(x - 1, y - 2)
(x + 2, y + 1)
(x + 2, y - 1)
(x - 2, y + 1)
(x - 2, y - 1)
```

## Example

```text
[[0,0,0],
 [0,0,0],
 [0,0,0]]
source = [2, 0] destination = [2, 2] return 2

[[0,1,0],
 [0,0,0],
 [0,0,0]]
source = [2, 0] destination = [2, 2] return 6

[[0,1,0],
 [0,0,1],
 [0,0,0]]
source = [2, 0] destination = [2, 2] return -1
```


# Walls and Gates

## Description

You are given a m x n 2D grid initialized with these three possible values.

1. `-1` - A wall or an obstacle.
2. `0` - A gate.
3. `INF` - Infinity means an empty room. We use the value `231 - 1 = 2147483647` to represent `INF` as you may assume that the distance to a gate is less than `2147483647`.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with `INF`.

## Example

Given the 2D grid:

```text
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
```

After running your function, the 2D grid should be:

```text
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
```

## Solution

从每个出口做BFS，然后，在每个位置取最小值，时间复杂度为O\(m \* n \* k\)。

然而，这不是最优的，可以先扫一遍，把所有的gate放进队列，然后做BFS，这个过程当中，记录位置是否访问过, 因为是按层遍历，每个先被访问过的点一定是最小值，然后时间复杂度是O\(m\*n\), 厉害了。

跳过。


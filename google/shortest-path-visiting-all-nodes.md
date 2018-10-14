# Shortest Path Visiting All Nodes

## Description

An undirected, connected graph of N nodes \(labeled `0, 1, 2, ..., N-1`\) is given as `graph`.

`graph.length = N`, and `j != i` is in the list `graph[i]` exactly once, if and only if nodes `i` and `j` are connected.

Return the length of the shortest path that visits every node. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.

## Example

**Example 1:**

```text
Input: [[1,2,3],[0],[0],[0]]
Output: 4
Explanation: One possible path is [1,0,2,0,3]
```

**Example 2:**

```text
Input: [[1],[0,2,4],[1,3,4],[2],[1,2]]
Output: 4
Explanation: One possible path is [0,1,4,2,3]
```

**Note:**

1. `1 <= graph.length <= 12`
2. `0 <= graph[i].length < graph.length`

## Solution

卧槽，试想一下，这也是Google的题目哦，可见你还差得很远。。

这里突然想到，search问题的一个关键点就是如何定义状态，较好的状态定义能有效减少搜索空间的大小。

这样如何，从每个点出发做一次search，状态定义为（当前位置+已经访问过的节点）， 找最短路径，每次优先访问访问过的节点。。

看一下吧。。

OK，看了解法，定义的状态就是当前位置+已经访问过的节点（那这题是要比Race car简单的）

我日喽，他为了方便存储和快速计算，用bit hash的方法，另外他还用了dist\[\]\[\]矩阵表示了每个状态到临近节点的最短距离，有种Dijkstra的意味在里面，待会儿再看看花花的讲解。






# Shortest Distance from All Buildings

## Description

You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values **0**, **1** or **2**, where:

* Each **0** marks an empty land which you can pass by freely.
* Each **1** marks a building which you cannot pass through.
* Each **2** marks an obstacle which you cannot pass through.

## Example

```text
Input: [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]

1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0

Output: 7 

Explanation: Given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2),
             the point (1,2) is an ideal empty land to build a house, as the total 
             travel distance of 3+3+1=7 is minimal. So return 7.
```

## Solution

似曾相识的题目，计算最短路径本身，应该会要去考虑使用BFS，因此一个Brute Force的方法，就是将每一个Empty Position作为起点，然后做BFS，计算其到所有的建筑物的总和。

当然，考虑到建筑物的数量较少的话，可以从每个建筑物出发， 做DFS，这样的话，做的DFS的次数可能会少些，然后在每个0的位置，统计到所有建筑物的距离和，当然，还要保证每个被计算的0处，到所有的建筑物都是可达的。

跳过，不写了


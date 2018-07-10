---
description: 超经典的题，面试碰到的概率很高
---

# Word Ladder

## Description

Given two words \(start and end\), and a dictionary, find the length of shortest transformation sequence from startto end, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary

## Example

Given:  
start = `"hit"`  
end = `"cog"`  
dict = `["hot","dot","dog","lot","log"]`

As one shortest transformation is `"hit" -> "hot" -> "dot" -> "dog" -> "cog"`,  
return its length `5`.

## Solution

一般而言，问最短路径，除了BFS之外，还有可能是动态规划

这道题基本思路较为简单，关键是如何构建graph。




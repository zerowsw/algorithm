# Max Points on a Line

## Description

Given _n_ points on a 2D plane, find the maximum number of points that lie on the same straight line.

**Example 1:**

```text
Input: [[1,1],[2,2],[3,3]]
Output: 3
Explanation:
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4
```

**Example 2:**

```text
Input: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
Output: 4
Explanation:
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6
```

## Solution

基本上我有两种思路，一种是两两确定一条直线，每条直线作为一个group，然后确定每个group的size，找到size最大的group。




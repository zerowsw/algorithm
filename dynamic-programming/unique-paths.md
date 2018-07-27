# Unique Paths

## Description

A robot is located at the top-left corner of a _m_ x _n_ grid.

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid.

How many possible unique paths are there?

## Example

Given m = `3` and n = `3`, return `6`.  
Given m = `4` and n = `5`, return `35`.

## Solution

这个的话，作为一条最基础的DP，一行行往下推即可。（这里就不用数学方法了）

直接的方法，用一个二维数组存储路径。当然，实际上可以只用一个一维数组存储中间结果。

```java
public class Solution {
    /**
     * @param m: positive integer (1 <= m <= 100)
     * @param n: positive integer (1 <= n <= 100)
     * @return: An integer
     */
    public int uniquePaths(int m, int n) {
        // write your code here
        int[] paths = new int[n];
        for (int i = 0; i < n; i++) {
            paths[i] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                paths[j] = paths[j - 1] + paths[j];
            }
        }
        return paths[n - 1];
    }
}
```


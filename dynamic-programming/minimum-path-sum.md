---
description: 练手题
---

# Minimum Path Sum

## Description

Given a **m x n** grid filled with non-negative numbers, find a path from top left to bottom right which **minimizes** the sum of all numbers along its path.

## Solution

同样的思路，用一维数组记录最小path sum即可。

```java
public class Solution {
    /**
     * @param grid: a list of lists of integers
     * @return: An integer, minimizes the sum of all numbers along its path
     */
    public int minPathSum(int[][] grid) {
        // write your code here
        int[] min_sum = new int[grid[0].length];
        
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (i == 0) {
                    if (j == 0) {
                        min_sum[0] = grid[0][0];
                        continue;
                    }
                    min_sum[j] = min_sum[j - 1] + grid[0][j]; 
                    continue;
                }
                if (j == 0) {
                    min_sum[0] = min_sum[0] + grid[i][0];
                } else {
                    min_sum[j] = grid[i][j] + Math.min(min_sum[j], min_sum[j - 1]); 
                }
            }
        }
        return min_sum[grid[0].length - 1];
    }
}
```






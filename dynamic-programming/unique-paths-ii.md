# Unique Paths II

## Description

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as `1` and `0` respectively in the grid.

## Example

For example,

There is one obstacle in the middle of a 3x3 grid as illustrated below.

```text
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```

The total number of unique paths is `2`.

## Solution

稍微想一想就可以明白，相较于之前的Unique Path，只要在遇到obstacles的时候，置0即可。

同样，中间结果使用一维数组存储。

因为横竖边界的问题，逻辑有些混乱， 可以参考九章的答案，包括记忆化搜索的部分：

[https://www.jiuzhang.com/solution/unique-paths-ii/](https://www.jiuzhang.com/solution/unique-paths-ii/)

这是我的答案：

```java
public class Solution {
    /**
     * @param obstacleGrid: A list of lists of integers
     * @return: An integer
     */
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        // write your code here
        if (obstacleGrid == null || obstacleGrid.length == 0 || obstacleGrid[0].length == 0) return 0;
        int[] paths = new int[obstacleGrid[0].length];
        boolean blocked = false;
        boolean colblocked = false;
        
        for (int i = 0 ; i < obstacleGrid[0].length; i++) {
            if (blocked) {
                paths[i] = 0;
            } else if (obstacleGrid[0][i] == 0) {
                paths[i] = 1;
            } else {
                if (i == 0) colblocked = true;
                paths[i] = 0;
                blocked = true;
            }
        }
        
        for (int i = 1; i < obstacleGrid.length; i++) {
            for (int j = 0; j < obstacleGrid[0].length; j++) {
                if (j == 0) {
                    if (colblocked) {
                        paths[0] = 0;
                        continue;
                    }
                    if (obstacleGrid[i][j] == 1) {
                        colblocked = true;
                        paths[0] = 0;
                    } else {
                        paths[0] = 1;
                    }
                    continue;
                }
                
                if (obstacleGrid[i][j] == 1) {
                    paths[j] = 0;
                } else {
                    paths[j] = paths[j] + paths[j - 1];
                }
            }
        }
        return paths[paths.length - 1];
    }
}
```


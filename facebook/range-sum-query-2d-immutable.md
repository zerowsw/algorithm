# Range Sum Query 2D - Immutable

## Description

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner \(row1, col1\) and lower right corner \(row2, col2\).

## Example

```text
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```

## Solution

首先嘛，必然是DP，然后，定义的二维数组存啥呢？

感觉没啥好说的。。

我想的确实就是最优解了，打卡写一下吧。

它的解法比较好的地方在于加了一维，从而避免了边界的初始化。

```java
class NumMatrix {
    int[][] dp;
    public NumMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return;
        dp = new int[matrix.length + 1][matrix[0].length + 1];
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                dp[i + 1][j + 1] = matrix[i][j] + dp[i][j + 1] + dp[i + 1][j] - dp[i][j];
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return dp[row2 + 1][col2 + 1] - dp[row2 + 1][col1] - dp[row1][col2 + 1] + dp[row1][col1];
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```


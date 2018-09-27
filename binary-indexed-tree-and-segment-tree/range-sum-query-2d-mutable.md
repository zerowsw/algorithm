# Range Sum Query 2D - Mutable

## Description

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner \(row1, col1\) and lower right corner \(row2, col2\).

![Range Sum Query 2D](https://leetcode.com/static/images/courses/range_sum_query_2d.png)  
The above rectangle \(with the red border\) is defined by \(row1, col1\) = **\(2, 1\)** and \(row2, col2\) = **\(4, 3\)**, which contains sum = **8**.

## Example

**Example:**  


```text
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
update(3, 2, 2)
sumRegion(2, 1, 4, 3) -> 10
```

**Note:**

1. The matrix is only modifiable by the update function.
2. You may assume the number of calls to update and sumRegion function is distributed evenly.
3. You may assume that row1 ≤ row2 and col1 ≤ col2.

## Solution

非mutable的情况，直接用prefix  sum, 也就是DP的方法来做。预处理的时间复杂度为O\(mn\), 然后查询时间就是O\(1\)的？

然后有大量修改的情况下，我们想要优化修改的时间复杂度，也就是说从O\(n\)到O\(logn\), 那么很明显的又是一个Binary Indexed Tree的应用。。

问题的关键在于怎么把这个二维的矩阵转换成FenwickTree。

改造的方法很巧妙，int\[\]\[\] tree, 中的每一行都是一个树状数组，然后每一列又是在每行成为树状数组的基础上构建的树状数组，反正，空想的话，基本不太可能想得到。。

```java
class NumMatrix {

    int[][] clonedMatrix;
    FenwickTree tree;
    
    private class FenwickTree{
        int[][] tree;
        int m;
        int n;
        public FenwickTree(int m, int n) {
            tree = new int[m + 1][n + 1];
            this.m = m;
            this.n = n;
        }
        
        public void update(int x, int y, int delta) {
            for (int i = x; i <= m; i += lowBit(i)) {
                for (int j = y; j <= n; j += lowBit(j)) {
                    tree[i][j] += delta;
                }
            }
        }
        
        public int sum(int x, int y) {
            int sum = 0;
            for (int i = x; i > 0; i -= lowBit(i)) {
                for (int j = y; j > 0; j -= lowBit(j)) {
                    sum += tree[i][j];
                }
            }
            return sum;
        }
        
        private int lowBit(int x) {
            return x & (-x);
        }
    }
    
    public NumMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return;
        clonedMatrix = matrix.clone();
        tree = new FenwickTree(matrix.length, matrix[0].length);
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                tree.update(i + 1, j + 1, matrix[i][j]);
            }
        }
    }
    
    public void update(int row, int col, int val) {
        int delta = val - clonedMatrix[row][col];
        clonedMatrix[row][col] = val;
        tree.update(row + 1, col + 1, delta);
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return tree.sum(row2 + 1, col2 + 1) + tree.sum(row1, col1) - tree.sum(row2 + 1, col1) - tree.sum(row1, col2 + 1);
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * obj.update(row,col,val);
 * int param_2 = obj.sumRegion(row1,col1,row2,col2);
 */
```


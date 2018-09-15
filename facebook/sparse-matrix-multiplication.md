# Sparse Matrix Multiplication

## Description

Given two [sparse matrices](https://en.wikipedia.org/wiki/Sparse_matrix) **A** and **B**, return the result of **AB**.

You may assume that **A**'s column number is equal to **B**'s row number.

## Example

```text
Input:

A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]

Output:

     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
```

## Solution

这道题我做过，关键是怎么优化（这是稀疏矩阵，所以有很多的0）

我他么傻叉了。。。被嵌套的下标搞懵逼了。。

这是我的初始版本：

```java
class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        int[][] result = new int[A.length][B[0].length];
        for (int i = 0; i < A.length; i++) {
            for (int j = 0; j < A[0].length; j++) {
                for (int y = 0; y < B[0].length; y++) {
                    result[i][y] += A[i][j] * B[j][y];
                }
            }
        }
        return result;
    }
}
```

优化一步之后，从11% 提升到55%：

```java
class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        int[][] result = new int[A.length][B[0].length];
        for (int i = 0; i < A.length; i++) {
            for (int j = 0; j < A[0].length; j++) {
                if (A[i][j] == 0) continue;
                for (int y = 0; y < B[0].length; y++) {
                    result[i][y] += A[i][j] * B[j][y];
                }
            }
        }
        return result;
    }
}
```

稍微加了个判断，到75%了：

```java
class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        int[][] result = new int[A.length][B[0].length];
        for (int i = 0; i < A.length; i++) {
            for (int j = 0; j < A[0].length; j++) {
                if (A[i][j] == 0) continue;
                for (int y = 0; y < B[0].length; y++) {
                    if (B[j][y] != 0) result[i][y] += A[i][j] * B[j][y];
                }
            }
        }
        return result;
    }
}
```

我觉得这样就够了。。


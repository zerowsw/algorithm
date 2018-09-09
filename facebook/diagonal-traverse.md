# Diagonal Traverse

## Description

Given a matrix of M x N elements \(M rows, N columns\), return all elements of the matrix in diagonal order as shown in the below image.

## Example

```text
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output:  [1,2,4,7,5,3,6,8,9]
Explanation:
```

## Solution

这道题没啥特别的算法，就是模拟一下这个过程。

现在再来做的话，发现，真的没啥诶，，挺直白的啊，我的代码如下：

```java
class Solution {
    public int[] findDiagonalOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return new int[]{};
        int m = matrix.length;
        int n = matrix[0].length;
        int[] res = new int[m * n];
        int x = 0;
        int y = 0;
        int dir = 0;
        int index = 0;
        while (index < m * n) {
            res[index++] = matrix[x][y];
            if (dir == 0) {
                if (x > 0 && y < n - 1) {
                    x--;
                    y++;
                } else if (y == n - 1) {
                    x++;
                    dir = dir ^ 1;
                } else {
                    y++;
                    dir = dir ^ 1;
                }
            } else {
                if (y > 0 && x < m - 1) {
                    x++;
                    y--;
                } else if (x == m - 1) {
                    y++;
                    dir = dir ^ 1;
                } else {
                    x++;
                    dir = dir ^ 1;
                }
            }
        }
        return res;
    }
}
```


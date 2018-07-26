# Maximum Submatrix

## Description

Given an `n x n` matrix of `positive` and `negative` integers, find the submatrix with the largest possible sum.

## Example

```text
Given matrix = 
[
[1,3,-1],
[2,3,-2],
[-1,-2,-3]
]
return 9.
Explanation:
the submatrix with the largest possible sum is:
[
[1,2],
[2,3]
]
```

## Solution

一个brute force的方法，就是构造prefix sum的矩阵，在构造的同时，在当前点遍历所有可能的子矩阵，从而找出其中sum最大的一个。。。。。。

好吧，参考了答案。使用降维攻击，先枚举所有的上下边界，然后压缩成一维数组，在此基础上，算一次maximum  subarray即可。

这个思路在解决很多矩阵相关问题上非常用借鉴意义：

```java
public class Solution {
    /**
     * @param matrix: the given matrix
     * @return: the largest possible sum
     */
    public int maxSubmatrix(int[][] matrix) {
        // write your code here
        int max = Integer.MIN_VALUE;
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;
        
        int m = matrix.length;
        int n = matrix[0].length;
        
        for (int i = 0; i < m; i++) {
            for (int j = i; j < m; j++) {
                int[] array = compressed(matrix, i, j);
                int max_sum = maxSubarray(array);
                max = Math.max(max_sum, max);
            }
        }
        return max;
    }
    
    private int maxSubarray(int[] array) {
        int max = Integer.MIN_VALUE;
        int sum = 0;
        int minSum = 0;
        for (int i = 0; i < array.length; i++) {
            sum += array[i];
            max = Math.max(sum - minSum, max);
            minSum = Math.min(minSum, sum);
        }
        return max;
    }
    
    private int[] compressed(int[][] matrix, int lo, int hi) {
        int n = matrix[0].length;
        int[] res = new int[n];
        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = lo; j <= hi; j++) {
                sum += matrix[j][i];
            }
            res[i] = sum;
        }
        return res;
    }
}
```


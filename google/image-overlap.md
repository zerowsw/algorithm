# Image Overlap

## Description

Two images `A` and `B` are given, represented as binary, square matrices of the same size.  \(A binary matrix has only 0s and 1s as values.\)

We translate one image however we choose \(sliding it left, right, up, or down any number of units\), and place it on top of the other image.  After, the _overlap_ of this translation is the number of positions that have a 1 in both images.

\(Note also that a translation does **not** include any kind of rotation.\)

What is the largest possible overlap?

**Example 1:**

```text
Input: A = [[1,1,0],
            [0,1,0],
            [0,1,0]]
       B = [[0,0,0],
            [0,1,1],
            [0,0,1]]
Output: 3
Explanation: We slide A to right by 1 unit and down by 1 unit.
```

**Notes:** 

1. `1 <= A.length = A[0].length = B.length = B[0].length <= 30`
2. `0 <= A[i][j], B[i][j] <= 1`

## Solution

brute force 的方法是O\(n^6\)

如果反过来，用delta来统计的话，就是O\(n ^ 4\)

```java
class Solution {
    public int largestOverlap(int[][] A, int[][] B) {
        int N = A.length;
        int[][] delta = new int[2 * N + 1][2 * N + 1];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                if (A[i][j] == 1) {
                    for (int bi = 0; bi < N; bi++) {
                        for (int bj = 0; bj < N; bj++) {
                            if (B[bi][bj] == 1) {
                                delta[i - bi + N][j - bj + N]++;
                            }
                        }
                    }
                }
            }
        }
        int overlap = 0;
        for (int i = 0; i < delta.length; i++) {
            for (int j = 0; j < delta[0].length; j++) {
                overlap = Math.max(overlap, delta[i][j]);
            }
        }
        return overlap;
    }
}
```


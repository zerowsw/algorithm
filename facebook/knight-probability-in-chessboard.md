---
description: 其实这道题应该很自然地想到DP，不知道为啥脑子抽了。。
---

# Knight Probability in Chessboard

## Description

On an `N`x`N` chessboard, a knight starts at the `r`-th row and `c`-th column and attempts to make exactly `K` moves. The rows and columns are 0 indexed, so the top-left square is `(0, 0)`, and the bottom-right square is `(N-1, N-1)`.

A chess knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

![](../.gitbook/assets/knight.png)

Each time the knight is to move, it chooses one of eight possible moves uniformly at random \(even if the piece would go off the chessboard\) and moves there.

The knight continues moving until it has made exactly `K` moves or has moved off the chessboard. Return the probability that the knight remains on the board after it has stopped moving.

## Example

```text
Input: 3, 2, 0, 0
Output: 0.0625
Explanation: There are two moves (to (1,2), (2,1)) that will keep the knight on the board.
From each of those positions, there are also two moves that will keep the knight on the board.
The total probability the knight stays on the board is 0.0625.
```

## Note

`N` will be between 1 and 25.

`K` will be between 0 and 100.

The knight always initially starts on the board.

## Solution

卧槽，冷静一下，，

我觉得吧，直白一点的方法，就是check走完N步之后的所有位置？？ 

感觉会爆表啊。。

卧槽，居然是DP，是在下输了。。

直接看答案了。。

solution里面的dp记录的是概率，discussion里面的方法，记录的是次数，于我而言，记录次数更为直观。

以下是错误解法：\(没有考虑出去又回来的情况\)

```java
class Solution {
    public double knightProbability(int N, int K, int r, int c) {
        int[][] dp1= new int[N][N];
        dp1[r][c] = 1;
        int[][] dir = {{-2, 1}, {-2, -1}, {-1, 2}, {-1, -2}, {1, 2}, {1, -2}, {2, 1}, {2, -1}};
        for (int i = 0; i < K; i++) {
            int[][] dp2 = new int[N][N];
            for (int x = 0; x < N; x++) {
                for (int y = 0; y < N; y++) {
                    for (int[] d : dir) {
                        int newx = x + d[0];
                        int newy = y + d[1];
                        if (newx >= 0 && newx < N && newy >=0 && newy < N) {
                            dp2[newx][newy] += dp1[x][y];
                        }
                    }
                }
            }
            dp1 = dp2;
        }
        int count = 0;
        for (int[] sub_dp : dp1) {
            for (int s : sub_dp) {
                count += s;
            }
        }
        return (double)count / (double)Math.pow(8, K);
    }
}
```

傻了，，，你压根儿不用考虑出去又回来的情况，因为压根儿就不会出去。。

**我的错误在于没有考虑到溢出的问题，需要换成double。。。**

如下：

```java
class Solution {
    public double knightProbability(int N, int K, int r, int c) {
        int[][] dp1= new int[N][N];
        dp1[r][c] = 1;
        int[][] dir = {{-2, 1}, {-2, -1}, {-1, 2}, {-1, -2}, {1, 2}, {1, -2}, {2, 1}, {2, -1}};
        for (int i = 0; i < K; i++) {
            int[][] dp2 = new int[N][N];
            for (int x = 0; x < N; x++) {
                for (int y = 0; y < N; y++) {
                    for (int[] d : dir) {
                        int newx = x + d[0];
                        int newy = y + d[1];
                        if (newx >= 0 && newx < N && newy >=0 && newy < N) {
                            dp2[x][y] += dp1[newx][newy];
                        }
                    }
                }
            }
            dp1 = dp2;
        }
        int count = 0;
        for (int[] sub_dp : dp1) {
            for (int s : sub_dp) {
                count += s;
            }
        }
        return (double)(count / Math.pow(8, K));
    }
}
```

概率的解法：

```java
class Solution {
    public double knightProbability(int N, int K, int sr, int sc) {
        double[][] dp = new double[N][N];
        int[] dr = new int[]{2, 2, 1, 1, -1, -1, -2, -2};
        int[] dc = new int[]{1, -1, 2, -2, 2, -2, 1, -1};

        dp[sr][sc] = 1;
        for (; K > 0; K--) {
            double[][] dp2 = new double[N][N];
            for (int r = 0; r < N; r++) {
                for (int c = 0; c < N; c++) {
                    for (int k = 0; k < 8; k++) {
                        int cr = r + dr[k];
                        int cc = c + dc[k];
                        if (0 <= cr && cr < N && 0 <= cc && cc < N) {
                            dp2[cr][cc] += dp[r][c] / 8.0;
                        }
                    }
                }
            }
            dp = dp2;
        }
        double ans = 0.0;
        for (double[] row: dp) {
            for (double x: row) ans += x;
        }
        return ans;
    }
}
```


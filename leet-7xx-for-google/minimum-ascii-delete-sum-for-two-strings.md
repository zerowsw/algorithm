---
description: >-
  解题思路类似Longest Common Subsequence
  （https://www.geeksforgeeks.org/printing-longest-common-subsequence/）-  DP经典题
---

# Minimum ASCII Delete Sum for Two Strings

## Description

Given two strings `s1, s2`, find the lowest ASCII sum of deleted characters to make two strings equal.

**Example 1:**

```text
Input: s1 = "sea", s2 = "eat"
Output: 231
Explanation: Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum.
Deleting "t" from "eat" adds 116 to the sum.
At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.
```

**Example 2:**

```text
Input: s1 = "delete", s2 = "leet"
Output: 403
Explanation: Deleting "dee" from "delete" to turn the string into "let",
adds 100[d]+101[e]+101[e] to the sum.  Deleting "e" from "leet" adds 101[e] to the sum.
At the end, both strings are equal to "let", and the answer is 100+101+101+101 = 403.
If instead we turned both strings into "lee" or "eet", we would get answers of 433 or 417, which are higher.
```

**Note:**

`0 < s1.length, s2.length <= 1000`.

All elements of each string will have an ASCII value in `[97, 122]`.

## Solution

看完问题描述之后，其实就是找到两个String的最大的subsequence交集

直接排序处理的话想来也是不行的，毕竟我们需要考虑原本的相对顺序。

这种最大最小的问题，感觉用DFS，然后也许可以用cache来优化，或者用DP来实现。

貌似没人用DFS解决，我一开始用DFS的时候，想得是构建longest common subsequence, 结果一下子转不过来了。。（其实很简单啊，把ASCII的值放进去就行了）

然后，让我们来想一想DP的做法：

首先可以很简单的确定是二维的DP，dp\[i\]\[j\] 可表示 s1\[i : \] 和 s2\[j : \]的结果

我们有以下的地推关系式： dp\[i\]\[s1.length\(\)\]  = dp\[i + 1\]\[s1.length\(\)\] + s1.charAt\(i\)

当s1.charAt\(i\) == s2.charAt\(j\) 时，我们有dp\[i\]\[j\] = dp\[i + 1\]\[j + 1\];

当s1.charAt\(i\) != s2.charAt\(j\)时，我们有dp\[i\]\[j\] = Math.min\(dp\[i\]\[j + 1\], dp\[i + 1\]\[j\]\)

感觉是非常typical的一道DP题目，需要好好记住哦：

```java
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int[][] dp = new int[s1.length() + 1][s2.length() + 1];
        for (int i = s1.length() - 1; i >= 0; i--) {
            dp[i][s2.length()] = dp[i + 1][s2.length()] + s1.charAt(i); 
        }
        for (int j = s2.length() - 1; j >= 0; j--) {
            dp[s1.length()][j] = dp[s1.length()][j + 1] + s2.charAt(j);
        }
        
        for (int i = s1.length() - 1; i >= 0; i--) {
            for (int j = s2.length() - 1; j >= 0; j--) {
                if (s1.charAt(i) == s2.charAt(j)) {
                    dp[i][j] = dp[i + 1][j + 1];
                } else {
                    dp[i][j] = Math.min(dp[i + 1][j] + s1.charAt(i), dp[i][j + 1] + s2.charAt(j));
                }
            }
        }
        return dp[0][0];
    }
}
```






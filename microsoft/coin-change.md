# Coin Change

## Description

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

**Example 1:**

```text
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```text
Input: coins = [2], amount = 3
Output: -1
```

**Note**:  
You may assume that you have an infinite number of each kind of coin.

## Solution

首先嘛，搜索的方法肯定是可以做的。。不过嘛，这道题最经典的方法还是DP。

卧槽，我本来想的DFS + cache的方法，其实就是Top - down的DP：

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount < 0) return -1;
        return helper(coins, amount, new int[amount + 1]);
    }
    
    public int helper(int[] coins, int amount, int[] count) {
        if (amount == 0) return 0;
        if (count[amount] != 0) return count[amount];
        
        int c = Integer.MAX_VALUE;
        for (int coin : coins) {
            if (amount >= coin) {
                int r = helper(coins, amount - coin, count);
                if (r != -1) {
                    c = Math.min(c, r + 1);
                }
            }
        }
        count[amount] = c == Integer.MAX_VALUE ? -1 : c;
        return count[amount];
    }
}
```

* Time complexity : O\(S\*n\)O\(S∗n\). where S is the amount, n is denomination count. In the worst case the recursive tree of the algorithm has height of SS and the algorithm solves only SS subproblems because it caches precalculated solutions in a table. Each subproblem is computed with nn iterations, one by coin denomination. Therefore there is O\(S\*n\)O\(S∗n\) time complexity.
* Space complexity : O\(S\)O\(S\), where SS is the amount to change We use extra space for the memoization table.

Bottom up的DP：

其实做到现在，bottom up的DP也比较好想了。。

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (i >= coin) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        return dp[amount] == amount + 1 ? -1 : dp[amount];
    }
}
```

* Time complexity : O\(S\*n\)O\(S∗n\). On each step the algorithm finds the next _F\(i\)F\(i\)_ in nn iterations, where 1\leq i \leq S1≤i≤S. Therefore in total the iterations are S\*nS∗n.
* Space complexity : O\(S\)O\(S\). We use extra space for the memoization table.


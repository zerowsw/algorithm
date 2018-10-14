---
description: 我日了，这题不也是LIS（longest increasing subsequence的问题嘛。。）
---

# Largest Divisible Subset

## Description

Given a set of **distinct** positive integers, find the largest subset such that every pair \(Si, Sj\) of elements in this subset satisfies:

Si % Sj = 0 or Sj % Si = 0.

If there are multiple solutions, return any subset is fine.

**Example 1:**

```text
Input: [1,2,3]
Output: [1,2] (of course, [1,3] will also be ok)
```

**Example 2:**

```text
Input: [1,2,4,8]
Output: [1,2,4,8]
```

## Solution

我日了喽，我居然没想到是LIS的问题，，我醉了。。。。。

```java
class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        int[] dp = new int[nums.length];
        int[] path = new int[nums.length];
        for (int i = 0; i < path.length; i++) {
            path[i] = -1;
        }
        
        Arrays.sort(nums);
        int max = 0;
        int lastindex = -1;
        for (int i = 0; i < nums.length; i++) {
            dp[i] = 1;
            for (int j = i - 1; j >= 0; j--) {
                if (nums[i] % nums[j] == 0 && dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                    path[i] = j;
                }
            }
            if (dp[i] > max) {
                max = dp[i];
                lastindex = i;
            }
        }
        
        List<Integer> result = new LinkedList<>();
        while (lastindex != -1) {
            result.add(nums[lastindex]);
            lastindex = path[lastindex];
        }
        return result;
    }
}
```


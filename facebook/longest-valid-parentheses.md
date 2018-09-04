# Longest Valid Parentheses

## Description

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid \(well-formed\) parentheses substring.

## Example

**Example 1:**

```text
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

**Example 2:**

```text
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

## Solution

诶呀，这个递推关系式还是6的呀，DP数组存的是以该位置结尾的最长valid parentheses的长度。

想想这种情况怎么处理："\(\)\(\(\)\)"

好好看看这其中的思考：

```java
class Solution {
    public int longestValidParentheses(String s) {
        int[] dp = new int[s.length()];
        Arrays.fill(dp, 0);
        
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) == '(') continue;
            if (s.charAt(i - 1) == '(') {
                dp[i] = i >= 2 ? dp[i - 2] + 2 : 2;
            } else if (i - 1 - dp[i - 1] >= 0 && s.charAt(i - 1 - dp[i - 1]) == '(') {
                dp[i] = dp[i - 1] + 2;
            }
        }
        int max = 0;
        for (int v : dp) {
            max = Math.max(max, v);
        }
        return max;
    }
}
```


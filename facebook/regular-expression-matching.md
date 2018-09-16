# Regular Expression Matching

## Description

Given an input string \(`s`\) and a pattern \(`p`\), implement regular expression matching with support for `'.'` and `'*'`.

```text
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the **entire** input string \(not partial\).

**Note:**

* `s` could be empty and contains only lowercase letters `a-z`.
* `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

## Example

**Example 1:**

```text
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```text
Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```text
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**

```text
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
```

**Example 5:**

```text
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```

## Solution

看一看这一种情况， “aaaab“ 和 "a\*ab", 答案显然是True的，然后在匹配的时候，我们并不知道要匹配多少个。。所以，从这个角度来看，我们可能需要DFS

估摸着比较好的方法是用DP或者DFS with Memory

先是用Recursion的做法，这种每次处理一个的结构，很值得学习，然后处理corner case的时候，有很多需要注意的点，这是我的解法：

```java
class Solution {
    public boolean isMatch(String s, String p) {
        //end condition
        if (s == null || p == null) return false;
        if (s.length() == 0 && p.length() == 0) return true;
        if (s.length() == 0 && p.length() == 1) return false;
        if (p.length() == 0) return false;
        
        boolean match = false;
        if (s.length() > 0 && (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.')) {
            match = true;
        }
        if (p.length() > 1 && p.charAt(1) == '*') {
            return isMatch(s, p.substring(2)) || (match  && isMatch(s.substring(1), p));
        }
        return match && isMatch(s.substring(1), p.substring(1));
    }
}
```

稍微修改了一下：

```java
class Solution {
    public boolean isMatch(String s, String p) {
        //end condition
        if (s == null || p == null) return false;
        if (p.length() == 0) return s.length() == 0;
        
        boolean match = false;
        if (s.length() > 0 && (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.')) {
            match = true;
        }
        
        if (p.length() > 1 && p.charAt(1) == '*') {
            return isMatch(s, p.substring(2)) || (match  && isMatch(s.substring(1), p));
        }
        return match && isMatch(s.substring(1), p.substring(1));
    }
}
```

然后，分析一下时间复杂度，这个，实在无力，你自己看吧：[https://leetcode.com/problems/regular-expression-matching/solution/](https://leetcode.com/problems/regular-expression-matching/solution/)

DP的方法其实不难理解，尤其是自上而下的方法，给我点时间也能写出来，不过现在没时间搞了，这里直接粘我的代码了：

```java

class Solution {
    int[][] memo;

    public boolean isMatch(String text, String pattern) {
        memo = new int[text.length() + 1][pattern.length() + 1];
        return dp(0, 0, text, pattern);
    }

    public boolean dp(int i, int j, String text, String pattern) {
        if (memo[i][j] != 0) {
            return memo[i][j] == 1;
        }
        boolean ans;
        if (j == pattern.length()){
            ans = i == text.length();
        } else{
            boolean first_match = (i < text.length() &&
                                   (pattern.charAt(j) == text.charAt(i) ||
                                    pattern.charAt(j) == '.'));

            if (j + 1 < pattern.length() && pattern.charAt(j+1) == '*'){
                ans = (dp(i, j+2, text, pattern) ||
                       first_match && dp(i+1, j, text, pattern));
            } else {
                ans = first_match && dp(i+1, j+1, text, pattern);
            }
        }
        memo[i][j] = ans ? 1 : -1;
        return ans;
    }
}
```

时间复杂度，借鉴Target Sum的分析方法，应该是O\(TP\),  T, P分别是text 和 pattern的长度。


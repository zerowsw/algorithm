# Is Subsequence

## Description

Given a string **s** and a string **t**, check if **s** is subsequence of **t**.

You may assume that there is only lower case English letters in both **s** and **t**. **t** is potentially a very long \(length ~= 500,000\) string, and **s** is a short string \(&lt;=100\).

A subsequence of a string is a new string which is formed from the original string by deleting some \(can be none\) of the characters without disturbing the relative positions of the remaining characters. \(ie, `"ace"` is a subsequence of `"abcde"` while `"aec"` is not\).

**Example 1:**  
**s** = `"abc"`, **t** = `"ahbgdc"`

Return `true`.

**Example 2:**  
**s** = `"axc"`, **t** = `"ahbgdc"`

Return `false`.

**Follow up:**  
If there are lots of incoming S, say S1, S2, ... , Sk where k &gt;= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?

## Solution

懵逼了一下，按顺序扫就可以了：

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        if (s.length() == 0) return true;
        int sindex = 0;
        char cur = s.charAt(sindex);
        for (int i = 0; i < t.length(); i++) {
            if (t.charAt(i) == cur) {
                if (sindex == s.length() - 1) return true;
                sindex++;
                cur = s.charAt(sindex);
            }
        }
        return false;
    }
}
```


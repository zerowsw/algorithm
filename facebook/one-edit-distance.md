# One Edit Distance

## Description

Given two strings **s** and **t**, determine if they are both one edit distance apart.

**Note:** 

There are 3 possiblities to satisify one edit distance apart:

1. Insert a character into _**s**_ to get _**t**_
2. Delete a character from _**s**_ to get _**t**_
3. Replace a character of _**s**_ to get _**t**_

## Example

**Example 1:**

```text
Input: s = "ab", t = "acb"
Output: true
Explanation: We can insert 'c' into s to get t.
```

**Example 2:**

```text
Input: s = "cab", t = "ad"
Output: false
Explanation: We cannot get t from s by only one step.
```

**Example 3:**

```text
Input: s = "1203", t = "1213"
Output: true
Explanation: We can replace '0' with '1' to get t.
```

## Solution

毕竟只是One Edit Distance, 注意一下edge case就行了。。

我的解法还是老样子，忘了处理很多的edge case:

```java
class Solution {
    public boolean isOneEditDistance(String s, String t) {
        if (s == null || t == null) return false;
        if (s.equals(t)) return false;
        if (Math.abs(s.length() - t.length()) > 1) return false;
        
        char[] s_char = s.toCharArray();
        char[] t_char = t.toCharArray();
        int i = 0; 
        int j = 0;
        boolean skip = false;
        while (i < s_char.length && j < t_char.length) {
            if (s_char[i] == t_char[j]) {
                i++;
                j++;
                continue;
            } else if (skip) {
                return false;
            } else if (s_char.length == t_char.length) {
                skip = true;
                i++;
                j++;
            } else if (s_char.length < t_char.length) {
                j++;
                skip = true;
            } else {
                i++;
                skip = true;
            }
        }
        return true;
    }
}
```


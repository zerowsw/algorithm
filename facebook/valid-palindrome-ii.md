# Valid Palindrome II

## Description

Given a non-empty string `s`, you may delete **at most** one character. Judge whether you can make it a palindrome.

## Solution

这个问题提供的解答看起来稍微优美一点，下面的是我的解法：

```java
class Solution {
    
    boolean skip = false;
    public boolean validPalindrome(String s) {
        if (s == null || s.length() == 0) return false;
        int le = 0;
        int ri = s.length() - 1;
        return helper(s, le, ri);
    }
    
    public boolean helper(String s, int le, int ri) {
        while (le < ri) {
            if (s.charAt(le) == s.charAt(ri)) {
                le++;
                ri--;
            } else if (skip){
                return false;
            } else {
                skip = true;
                return helper(s, le + 1, ri) || helper(s, le, ri - 1);
            }
        }
        return true;
    }
}
```


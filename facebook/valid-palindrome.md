# Valid Palindrome

## Description

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note:** For the purpose of this problem, we define empty string as valid palindrome.

## Example

**Example 1:**

```text
Input: "A man, a plan, a canal: Panama"
Output: true
```

**Example 2:**

```text
Input: "race a car"
Output: false
```

## Solution

这道题本身没啥，主要是我忘了toLowerCase\(\)这个函数的使用，包括Character.toLowerCase\(\),  还有String的toLowerCase\(\)

```java
class Solution {
    public boolean isPalindrome(String s) {
        int le = 0;
        int ri = s.length() - 1;
        s = s.toLowerCase();
        while (le < ri) {
            while (le < ri && !Character.isLetterOrDigit(s.charAt(le))) {
                le++;
            }
            while (le < ri && !Character.isLetterOrDigit(s.charAt(ri))) {
                ri--;
            }
            if (s.charAt(le++) != s.charAt(ri--)) return false;
        }
        return true;
    }
}
```


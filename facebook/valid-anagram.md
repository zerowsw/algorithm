---
description: 这道题作为很多问题的子问题出现过，好好搞清楚比较好的解法。
---

# Valid Anagram

## Description

Given two strings _s_ and _t_ , write a function to determine if _t_ is an anagram of _s_.

## Example

**Example 1:**

```text
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```text
Input: s = "rat", t = "car"
Output: false
```

## Solution

在有重复性的情况下，可能并没有我所想象的那种时间O\(n\)， 同时空间也极其优化的方法。

用数组来统计是一个相对map更好些的方法。

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) return false;
        int[] cnt = new int[26];
        for (char chr : s.toCharArray()) {
            cnt[chr - 'a']++;
        }
        for (char chr : t.toCharArray()) {
            if (cnt[chr - 'a'] <= 0) return false;
            cnt[chr - 'a']--;
        }
        return true;
    }
}
```


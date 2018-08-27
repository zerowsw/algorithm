---
description: 还需要找其他sliding window的问题练练手！！！（参加解答链接里面的问题链接）
---

# Find All Anagrams in a String

## Description

Given a string **s** and a **non-empty** string **p**, find all the start indices of **p**'s anagrams in **s**.

Strings consists of lowercase English letters only and the length of both strings **s** and **p** will not be larger than 20,100.

The order of output does not matter.

## Example

**Example 1:**

```text
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```text
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

## Solution

比较Brute Force的方法，就是比较每个子序列， 子问题就是O\(n\)时间的valid anagram

时间复杂度是O\(mn\)

同时，你可以想象到还有比较明显的比较好的解法， 即sliding window

这里参考了解答的sliding window模版式解法，使用很多sliding window相关问题，并且简介易懂：

[https://leetcode.com/problems/find-all-anagrams-in-a-string/discuss/92015/ShortestConcise-JAVA-O\(n\)-Sliding-Window-Solution](https://leetcode.com/problems/find-all-anagrams-in-a-string/discuss/92015/ShortestConcise-JAVA-O%28n%29-Sliding-Window-Solution)

解法如下： \(仔细斟酌！！！\)

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        
        List<Integer> res = new ArrayList<>();
        if (s == null || p == null || s.length() == 0 || p.length() == 0) return res;
        
        int[] cnt = new int[26];
        for (char chr : p.toCharArray()) {
            cnt[chr - 'a']++;
        }
        
        int le = 0;
        int ri = 0;
        int count = 0;
        int size = p.length();
        while (ri < s.length()) {
            if (cnt[s.charAt(ri++) - 'a']-- >= 1) count++;
            if (count == size) res.add(le);
            if (ri - le == size && cnt[s.charAt(le++) - 'a']++ >= 0) count--; 
        }
        return res;
    }
}
```


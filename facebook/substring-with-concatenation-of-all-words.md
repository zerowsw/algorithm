---
description: 需要重新回复来体会一下这其中的思考过程
---

# Substring with Concatenation of All Words

## Description

You are given a string, **s**, and a list of words, **words**, that are all of the same length. Find all starting indices of substring\(s\) in **s** that is a concatenation of each word in **words** exactly once and without any intervening characters.

## Example

**Example 1:**

```text
Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoor" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```

**Example 2:**

```text
Input:
  s = "wordgoodstudentgoodword",
  words = ["word","student"]
Output: []
```

## Solution

妈勒个鸡，好像被卡住了。。

看了下topic，是Hash Table, two pointers, 算了，看答案把。。

没在leetcode上看到比较统一的解法，排名第一的太脑残了。。

我看看九章上面的解法：

其实和我想的brute force的方法是一致的，我没想到的是可以用另外一HashMap来check当前扫描找到的word。

其实，思路相当简洁明了。。  至于O\(n ^ 2\)的解法，我这里就不列出来了。。

```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        Map<String, Integer> tofind = new HashMap<>();
        List<Integer> result = new ArrayList<>();
        if (s == null || words == null || words.length == 0) return result;
        for (String word : words) {
            tofind.put(word, tofind.getOrDefault(word, 0) + 1);
        }
        int n = words.length;
        int len = words[0].length();
        for (int i = 0; i <= s.length() - n * len; i++) {
            Map<String, Integer> count = new HashMap<>();
            for (int j = i; j <= i + (n - 1) * len; j += len) {
                String cur = s.substring(j, j + len);
                if (!tofind.containsKey(cur)) break;
                count.put(cur, count.getOrDefault(cur, 0) + 1);
                if (count.get(cur) > tofind.get(cur)) break;
                if (j == i + (n - 1) * len) result.add(i);
            }
        }
        return result;
    }
}
```








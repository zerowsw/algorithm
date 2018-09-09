# Longest Substring Without Repeating Characters

## Description

Given a string, find the length of the **longest substring** without repeating characters.

## Example

**Example 1:**

```text
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", which the length is 3.
```

**Example 2:**

```text
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```text
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Solution

序号是3，那就是很久很久以前做过的喽？

一看就觉得用sliding window，难道那时候我就会用这个了？

被下标困住了一段时间，唉：

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] hash = new int[255];
        int le = 0; 
        int ri = 0;
        int max = Integer.MIN_VALUE;
        char[] array = s.toCharArray();
        while (ri < s.length()) {
            while (ri < s.length() && hash[array[ri] - ' ']++ == 0) {
                ri++;
            }
            max = Math.max(max, ri - le);
            while (le < ri && hash[array[le] - ' ']-- == 1) {
                le++;
            }
            le++;
            ri++;
        }
        return max == Integer.MIN_VALUE ? 0 : max;
    }
}
```

优化之后的sliding window：

[https://leetcode.com/problems/longest-substring-without-repeating-characters/solution/](https://leetcode.com/problems/longest-substring-without-repeating-characters/solution/)


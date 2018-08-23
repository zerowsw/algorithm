# Decode Ways

## Description

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```text
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a **non-empty** string containing only digits, determine the total number of ways to decode it.

## Example

**Example 1:**

```text
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**

```text
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

## Solution

这道题应该做过，比较明显的Dynamic Programming问题。

这里在初始化的赋值上纠结了太多的时间，一开始没有搞清它的含义。然后，在值的递推关系上，也花费了一定的时间。

```java
class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) return 0;
        
        int prevTwo = 1;
        int prevOne = 1;
        if (s.charAt(0) == '0') {
            return 0;
        }
        
        for (int i = 1; i < s.length(); i++) {
            int two = Integer.parseInt(s.substring(i - 1, i + 1));
            if (s.charAt(i - 1) == '0' || two > 26 || two <= 0) {
                prevTwo = prevOne;
                if (s.charAt(i) == '0') return 0;
            } else if (s.charAt(i) == '0') {
                int temp = prevOne;
                prevOne = prevTwo;
                prevTwo = temp;
            } else {
                int temp = prevOne;
                prevOne = prevOne + prevTwo;
                prevTwo = temp;
            }
        }
        return prevOne;
    }
}
```


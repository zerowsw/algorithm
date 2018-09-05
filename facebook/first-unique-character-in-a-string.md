# First Unique Character in a String

## Description

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

## Example

```text
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```

## Solution

有时候，这样简单的问题，也会被莫名其妙地卡住。

一度以为，有啥惊天动地的解法，原来就是这样做。。

```text
class Solution {
    public int firstUniqChar(String s) {
        int[] count = new int[26];
        for (char chr : s.toCharArray()) {
            count[chr - 'a']++;
        }
        for (int i = 0; i < s.length(); i++) {
            if (count[s.charAt(i) - 'a'] == 1) return i;
        }
        return -1;
    }
}
```


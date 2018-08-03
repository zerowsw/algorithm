---
description: easy题，但我还是卡住了。。
---

# Count and Say

## Description

The count-and-say sequence is the sequence of integers with the first five terms as following:

```text
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` is read off as `"one 1"` or `11`.  
`11` is read off as `"two 1s"` or `21`.  
`21` is read off as `"one 2`, then `one 1"` or `1211`.  


Given an integer n, generate the nth term of the count-and-say sequence.

Note: Each term of the sequence of integers will be represented as a string.

## Solution

比较直接的easy题，关键是把问题好好想清楚。。

这里犯了个错误，integer + char 结果是char 。。。。

```java
class Solution {
    public String countAndSay(int n) {
        String str = "1";
        for (int i = 1; i < n; i++) {
            StringBuilder builder = new StringBuilder();
            int count = 0;
            for (int j = 0; j < str.length(); j++) {
                if (j > 0 && str.charAt(j) != str.charAt(j - 1)) {
                    builder.append(count + "" + str.charAt(j - 1));
                    count = 0;
                }
                count++;
            }
            builder.append(count + "" + str.charAt(str.length() - 1));
            str = builder.toString();
        }
        return str;
    }
}
```


# Add Binary

## Description

Given two binary strings, return their sum \(also a binary string\).

The input strings are both **non-empty** and contains only characters `1` or `0`.

## Example

**Example 1:**

```text
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```text
Input: a = "1010", b = "1011"
Output: "10101"
```

## Solution

没啥好说的，努力提高accept率，现阶段太垃圾了。

```java
class Solution {
    public String addBinary(String a, String b) {
        int pA = a.length() - 1;
        int pB = b.length() - 1;
        int t = 0;
        StringBuilder builder = new StringBuilder();
        while (pA >= 0 || pB >= 0) {
            int va = pA >= 0 ? a.charAt(pA) - '0' : 0;
            int vb = pB >= 0 ? b.charAt(pB) - '0' : 0;
            if (va + vb + t == 3) {
                builder.append("1");
            } else if (va + vb + t == 2) {
                builder.append("0");
                t = 1;
            } else {
                builder.append(va + vb + t);
                t = 0;
            }
            pA--;
            pB--;
        }
        if (t == 1) {
            builder.append(t);
        }
        return builder.reverse().toString();
    }
}
```


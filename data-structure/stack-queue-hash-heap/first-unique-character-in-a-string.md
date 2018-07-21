# First Unique Character in a String

## Description

Find the first unique character in a given string. You can assume that there is at least one unique character in the string.

## Example

For `"abaccdeff"`, return `'b'`.

## Challenge

no extra space

## Solution

你思考速度越来越慢了。。

首先，借助数据结构，空间换时间。 复杂度O\(n\)

其次，no extra space的话，没啥好想的，从左到右依次check。复杂度O\(n^2\)。（循环时也是从头扫到尾）

no extra:

```java
public class Solution {
    /**
     * @param str: str: the given string
     * @return: char: the first unique character in a given string
     */
    public char firstUniqChar(String str) {
        // Write your code here
        
        for(int i = 0; i < str.length(); i++) {
            char cur = str.charAt(i);
            boolean unique = true;
            for (int j = 0; j < str.length(); j++) {
                if (j == i) continue;
                if (str.charAt(j) == cur) {
                    unique = false;
                    break;
                }
            }
            if (unique) return cur;
        }
        return '0';
    }
}
```




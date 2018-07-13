---
description: 简单题，打个卡而已
---

# Letter Combinations of a Phone Number

## Description

Given a digit string excluded `01`, return all possible letter combinations that the number could represent.

A mapping of digit to letters \(just like on the telephone buttons\) is given below.

![Cellphone](https://lintcode-media.s3.amazonaws.com/problem/200px-Telephone-keypad2.svg.png)

## Example

Given `"23"`

Return `["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]`

## Solution

毫无难度。。

注意边界条件的判断

另外有个稍微可以改进的地方，就是存储的时候不需要存Int, 白白来回转化了两次。直接用char即可。

```java
public class Solution {
    /**
     * @param digits: A digital string
     * @return: all posible letter combinations
     */
    public List<String> letterCombinations(String digits) {
        // write your code here
        List<String> res = new ArrayList<>();
        if (digits == null || digits.length() == 0) {
            return res;
        }
        
        Map<Integer, char[]> map = new HashMap<>();
        map.put(2, new char[]{'a', 'b', 'c'});
        map.put(3, new char[]{'d', 'e', 'f'});
        map.put(4, new char[]{'g', 'h', 'i'});
        map.put(5, new char[]{'j', 'k', 'l'});
        map.put(6, new char[]{'m', 'n', 'o'});
        map.put(7, new char[]{'p', 'q', 'r', 's'});
        map.put(8, new char[]{'t', 'u', 'v'});
        map.put(9, new char[]{'w', 'x', 'y', 'z'});
        
        helper(res, "", map, digits, 0);
        return res;
    }
    
    public void helper(List<String> res, String cur, Map<Integer, char[]> map, String digits, int index) {
        if (index >= digits.length()) {
            res.add(cur);
            return;
        }
        
        for (Character chr : map.get(Integer.parseInt(digits.charAt(index) +""))) {
            helper(res, cur + chr, map, digits, index + 1);
        }
    }
}
```


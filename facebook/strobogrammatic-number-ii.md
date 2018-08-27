# Strobogrammatic Number II

## Description

A strobogrammatic number is a number that looks the same when rotated 180 degrees \(looked at upside down\).

Find all strobogrammatic numbers that are of length = n.

## Example

```text
Input:  n = 2
Output: ["11","69","88","96"]
```

## Solution

类似palindrome, 需要事先想一想可行的数字对，包括{{1, 1}, {6, 9}, {9, 6}, {8, 8} , {0, 0}}

妈蛋，不是check，是find all。。。。

那么，就是DFS，从中间往两侧构造（或者反过来）

注意中间的数字只能是0，1，8， 其他就没啥好说的了。

```java
class Solution {
    
    String[][] pairs = {{"1", "1"}, {"6", "9"}, {"9", "6"}, {"8", "8"}, {"0", "0"}};
    String[] center = {"0", "1", "8"};
    
    public List<String> findStrobogrammatic(int n) {
        
        List<String> res = new ArrayList<>();
        if (n % 2 == 0) {
            helper(res, "", n);
        } else {
            //核心数字只能是0， 1， 8
            for (String single : center){
                helper(res, single, n - 1);
            }
        }
        return res;
    }
    
    public void helper(List<String> res, String cur, int n) {
        if (n == 0) {
            res.add(cur);
            return;
        }
        
        if (n > 2) {
            for (String[] pair : pairs) {
                helper(res, pair[0] + cur + pair[1], n - 2);
            }
        } else {
            for (int i = 0; i < pairs.length - 1; i++) {
                helper(res, pairs[i][0] + cur + pairs[i][1], n - 2);
            }
        }
    }
}
```


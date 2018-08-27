# Custom Sort String

## Description

`S` and `T` are strings composed of lowercase letters. In `S`, no letter occurs more than once.

`S` was sorted in some custom order previously. We want to permute the characters of `T` so that they match the order that `S` was sorted. More specifically, if `x` occurs before `y` in `S`, then `x` should occur before `y` in the returned string.

Return any permutation of `T` \(as a string\) that satisfies this property.

## Example

```text
Example :
Input: 
S = "cba"
T = "abcd"
Output: "cbad"
Explanation: 
"a", "b", "c" appear in S, so the order of "a", "b", "c" should be "c", "b", and "a". 
Since "d" does not appear in S, it can be at any position in T. "dcba", "cdba", "cbda" are also valid outputs.
```

## Solution

想了下， 一个比较直白的做法，就是先用HashMap或者说hash array统计T中各字母的数量，然后按顺序遍历S中的字母，同时构造最终的字符串。

leetcode提供的解法也是一样的思路（虽然它用的是hash array），简称count and write。

其实思路上还是较为直接的，希望下次能够以更快的速度反应过来。

```java
class Solution {
    public String customSortString(String S, String T) {
        Map<Character, Integer> map = new HashMap<>();
        for (Character chr : T.toCharArray()) {
            map.put(chr, map.getOrDefault(chr, 0) + 1);
        }
        StringBuilder builder = new StringBuilder();
        for (Character chr : S.toCharArray()) {
            if (map.containsKey(chr)) {
                builder.append(constructString(chr, map.get(chr)));
                map.remove(chr);
            }
        }
        for (Map.Entry<Character, Integer> entry : map.entrySet()) {
            builder.append(constructString(entry.getKey(), entry.getValue()));
        }
        return builder.toString();
    }
    
    private String constructString(char chr, int count) {
        StringBuilder builder = new StringBuilder();
        for (int i = 0; i < count; i++) {
            builder.append(chr);
        }
        return builder.toString();
    }
}
```


# Minimum Window Substring

## Description

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O\(n\).

## Example

```text
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

## Note

* If there is no such window in S that covers all characters in T, return the empty string `""`.
* If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

## Solution

算是比较标准的slide window的题目，和我在pramp上碰到的那题不一样，这一题是有duplicates的。然后一个比较经典的数据结构就是用Char的int值作为下标的数组，小写或大写字母可以用26的size，全部ASCII码的话，可以用126的大小。

在实际写的过程中，需要思考的地方就是在移动指针时，在对应的时刻改变count的数量。

```java
class Solution {
    
    public String minWindow(String s, String t) {    
        int min = Integer.MAX_VALUE;
        String res = "";
        Set<Character> set = new HashSet<>();
        int[] letter = new int[126];
        int size = t.length();
        for (Character chr : t.toCharArray()) {
            set.add(chr);
            letter[chr]++;
        }
        
        int le = 0;
        int ri = 0;
        int count = 0;
        while (ri < s.length()) {
            if (set.contains(s.charAt(ri))) {
                if (letter[s.charAt(ri)] > 0) count++;
                letter[s.charAt(ri)]--;
            }       
            while (count == size && le <= ri) {
                if (ri - le + 1 < min) {
                    min = ri - le + 1;
                    res = s.substring(le, ri + 1);
                }
                if (set.contains(s.charAt(le))) letter[s.charAt(le)]++;
                if (letter[s.charAt(le)] > 0) count--;
                le++;
            }
            ri++;
        }
        return res;
    }
}
```


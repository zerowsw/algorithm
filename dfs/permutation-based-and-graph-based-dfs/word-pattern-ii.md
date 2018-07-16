# Word Pattern II

## Description

Given a `pattern` and a string `str`, find if `str` follows the same pattern.

Here **follow** means a full match, such that there is a [bijection](https://baike.baidu.com/item/%E5%8F%8C%E5%B0%84/942799?fr=aladdin) between a letter in `pattern` and a **non-empty**substring in `str`.\(i.e if `a` corresponds to `s`, then `b` cannot correspond to `s`. For example, given pattern = `"ab"`, str = `"ss"`, return `false`.\)

## Example

Given pattern = `"abab"`, str = `"redblueredblue"`, return `true`.  
Given pattern = `"aaaa"`, str = `"asdasdasdasd"`, return `true`.  
Given pattern = `"aabb"`, str = `"xyzabcxzyabc"`, return `false`.

## Solution

这道题与word pattern I 不同，需要尝试所有可能的映射关系，从而作出最后的判断。

我的解法空间溢出了，，不明原因：

```java
public class Solution {
    /**
     * @param pattern: a string,denote pattern string
     * @param str: a string, denote matching string
     * @return: a boolean
     */
    
    boolean result = false;
    public boolean wordPatternMatch(String pattern, String str) {
        // write your code here
        Map<Character, String> mapping = new HashMap<>();
        helper(mapping, pattern, str, 0, 0);
        return result;
    }
    
    private void helper(Map<Character, String> mapping, String pattern, String str, int indexP, int indexS){
        //end condition
        if (indexP == pattern.length() && indexS == str.length()) {
            result = true;
            return;
        } else if (indexP == pattern.length() || indexS == str.length()) {
            return;
        }
        
        char p = pattern.charAt(indexP);
        if (mapping.containsKey(p)) {
            String value = mapping.get(p);
            if (!checkP(value, str, indexS)) return;
            helper(mapping, pattern, str, indexP + 1, indexS + value.length());
        } else {
            for (int i = indexS + 1; i <= str.length(); i++) {
                String potential = str.substring(indexS, i);
                if (mapping.containsValue(potential)) break;
                mapping.put(p, potential);
                helper(mapping, pattern, str, indexP + 1, i);
                mapping.remove(p);
            }
        }
        
    }
    
    private boolean checkP(String p, String str, int start) {
        int len = p.length();
        //需要考虑越界的情况。
        if (len + start > str.length()) return false;
        for (int i = 0; i < len; i++) {
            if (p.charAt(i) != str.charAt(start + i)) return false;
        }
        return true;
    }
}
```



这是答案解法：

```java
public boolean wordPatternMatch(String pattern, String str) {
        Map<Character, String> map = new HashMap<>();
        Set<String> set = new HashSet<>();
        return match(pattern, str, map, set);
    }
    
    private boolean match(String pattern,
                          String str,
                          Map<Character, String> map,
                          Set<String> set) {
        if (pattern.length() == 0) {
            return str.length() == 0;
        }
        
        Character c = pattern.charAt(0);
        if (map.containsKey(c)) {
            if (!str.startsWith(map.get(c))) {
                return false;
            }
            
            return match(
                pattern.substring(1),
                str.substring(map.get(c).length()),
                map,
                set
            );
        }
        
        for (int i = 0; i < str.length(); i++) {
            String word = str.substring(0, i + 1);
            if (set.contains(word)) {
                continue;
            }
            map.put(c, word);
            set.add(word);
            if (match(pattern.substring(1),
                      str.substring(i + 1),
                      map,
                      set)) {
                return true;              
            }
            set.remove(word);
            map.remove(c);
        }
        
        return false;
    }
```


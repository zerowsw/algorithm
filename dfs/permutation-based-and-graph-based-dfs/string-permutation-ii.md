# String Permutation II

## Description

Given a string, find all permutations of it without duplicates.

## Example

Given `"abb"`, return `["abb", "bab", "bba"]`.

Given `"aabb"`, return `["aabb", "abab", "baba", "bbaa", "abba", "baab"]`.

## Solution

本来以为去重是个问题，其实也就这样。九章提供的算法比较独特，没有细看：[https://www.jiuzhang.com/solution/string-permutation-ii/](https://www.jiuzhang.com/solution/string-permutation-ii/)

```java
public class Solution {
    /**
     * @param str: A string
     * @return: all permutations
     */
    public List<String> stringPermutation2(String str) {
        // write your code here
        List<String> result = new ArrayList<>();
        Map<Character, Integer> map = new HashMap<>();
        for (Character chr : str.toCharArray()) {
            map.put(chr, map.getOrDefault(chr, 0) + 1);
        }
        helper(result, map, "", str.length());
        return result;
    }
    
    public void helper(List<String> result, Map<Character, Integer> map, String cur, int len) {
        
        if (cur.length() == len) {
            result.add(cur);
        }
        
        for (Map.Entry<Character, Integer> entry : map.entrySet()) {
            if (entry.getValue() == 0) continue;
            char key = entry.getKey();
            int value = entry.getValue();
            map.put(key, value - 1);
            helper(result, map, cur + key, len);
            map.put(key, value);
        }
    }
}
```




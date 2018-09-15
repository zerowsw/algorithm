# Group Anagrams

## Description

Given an array of strings, group anagrams together.

## Example

```text
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

## Solution

这道题的关键又是在于怎么样找到Anagram的有效表达方式。

看了一个，和我想的两个方法完全一直，一个是将String搞成char array 排序之后作为标志，一个是统计每个字母的数量，然后加起来作为标志（类似counting sort，所以更快）

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs.length == 0) return new ArrayList();
        Map<String, List> ans = new HashMap<String, List>();
        for (String s : strs) {
            char[] ca = s.toCharArray();
            StringBuilder builder = new StringBuilder();
            int[] array = new int[26];
            for (char chr : ca) {
                array[chr - 'a']++;
            }
            for (int i = 0; i < 26; i++) {
                builder.append(array[i] + "#");
            }
            String key = builder.toString();
            if (!ans.containsKey(key)) ans.put(key, new ArrayList());
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }
}
```


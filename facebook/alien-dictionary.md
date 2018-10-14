---
description: 快速简洁有效地构建graph， 又是一道仿佛能感觉到思想在流淌的题目，多写写。
---

# Alien Dictionary

Description

There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of **non-empty** words from the dictionary, where **words are sorted lexicographically by the rules of this new language**. Derive the order of letters in this language.

## Note

1. You may assume all letters are in lowercase.
2. You may assume that if a is a prefix of b, then a must appear before b in the given dictionary.
3. If the order is invalid, return an empty string.
4. There may be multiple valid order of letters, return any one of them is fine.

## Example

**Example 1:**

```text
Input:
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]

Output: "wertf"
```

**Example 2:**

```text
Input:
[
  "z",
  "x"
]

Output: "zx"
```

**Example 3:**

```text
Input:
[
  "z",
  "x",
  "z"
] 

Output: "" 

Explanation: The order is invalid, so return "".
```

## Solution

你他么是怎么想的？怎么会理解成单词里面的字母是按顺序排的呢？

在获取字母先后关系时，可以看出，整体的结构应该使用递归来实现。

卧槽，早知道不看topic了，其实感觉得出来应该是使用graph的来处理。不过，我被一些离散点的处理分散了注意了。其实，这是非常明显的topological sort。

搞清楚之后就比较好做了。

我构造graph的方法不太好，累赘代码太多，打算参考一下答案的写法。

有这样几个反思点：1. 首先，可以用HashMap表示图，没有必要建立新的节点类。 2. 构建图的时候，需要获取字母之间的先后关系，考虑到是拓扑排序，这里只需要把所有相邻的两个word中字母的先后顺序处理好就可以了。

```java
class Solution {
    public String alienOrder(String[] words) {
        
        String res = "";
        if (words == null || words.length == 0) return res;
        Map<Character, Set<Character>> graph = new HashMap<>();
        Map<Character, Integer> indegree = new HashMap<>();
        
        for (String str : words) {
            for (Character chr : str.toCharArray()) {
                indegree.put(chr, 0);
            }
        }
        
        for (int i = 0; i < words.length - 1; i++) {
            String a = words[i];
            String b = words[i + 1];
            int len = Math.min(a.length(), b.length());
            for (int j = 0; j < len; j++) {
                char ac = a.charAt(j);
                char bc = b.charAt(j);
                if (ac != bc) {
                    Set<Character> set = graph.getOrDefault(ac, new HashSet<>());
                    //这里记得不要重复计算。。
                    if (!set.contains(bc)) {
                        set.add(bc);
                        graph.put(ac, set);
                        indegree.put(bc, indegree.get(bc) + 1);
                    }
                    break;
                }
            }
        }
        
        Queue<Character> queue = new LinkedList<>();
        for (char chr : indegree.keySet()) {
            if (indegree.get(chr) == 0) queue.offer(chr);
        }
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                char cur = queue.poll();
                res += cur;
                //这里需要判断一下
                if (!graph.containsKey(cur)) continue;
                for (Character chr : graph.get(cur)) {
                    indegree.put(chr, indegree.get(chr) - 1);
                    if (indegree.get(chr) == 0) queue.offer(chr);
                }
            }
        }
        if (res.length() != indegree.size()) return "";
        return res;
    }
}
```


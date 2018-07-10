---
description: 超经典的题，面试碰到的概率很高
---

# Word Ladder

## Description

Given two words \(start and end\), and a dictionary, find the length of shortest transformation sequence from startto end, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary

## Example

Given:  
start = `"hit"`  
end = `"cog"`  
dict = `["hot","dot","dog","lot","log"]`

As one shortest transformation is `"hit" -> "hot" -> "dot" -> "dog" -> "cog"`,  
return its length `5`.

## Solution

一般而言，问最短路径，除了BFS之外，还有可能是动态规划

这道题基本思路较为简单，关键是如何构建graph。

错，大错特错，压根儿不需要构建graph，直接从初始word开始遍历即可。

以下是极蠢的做法：

```java
public class Solution {
    /*
     * @param start: a string
     * @param end: a string
     * @param dict: a set of string
     * @return: An integer
     */
    public int ladderLength(String start, String end, Set<String> dict) {
        // write your code here
        
        Queue<String> queue = new LinkedList<>();
        Set<String> set = new HashSet<>();
        queue.offer(start);
        set.add(start);
        dict.add(end);
        
        int level = 0;
        while (!queue.isEmpty()) {
            int s = queue.size();
            level++;
            for (int i = 0; i < s; i++) {
                String cur = queue.poll();
                if (cur.equals(end)) return level;
                List<String> next = helper(cur);
                for (String str : next) {
                    if (!dict.contains(str)) continue;
                    if (set.contains(str)) continue;
                    queue.offer(str);
                    set.add(str);
                }
            }
        }
        return -1;
    }
    
    public List<String> helper(String input) {
        char[] cur = input.toCharArray();
        int l = cur.length;
        List<String> res = new ArrayList<>();
        for (int i = 0; i < l; i++) {
            char temp = cur[i];
            for (int j = 0; j < 26; j++) {
                //char temp = cur[i];
                cur[i] = (char)('a' + j);
                res.add(String.valueOf(cur));
                //cur[i] = temp;
            }
            cur[i] = temp;
        }
        return res;
    }
}
```

实际上，每次从dict里面找，check是否距离为1，要高效得多：

额，75%的例子超时了。。

```java
public class Solution {
    /*
     * @param start: a string
     * @param end: a string
     * @param dict: a set of string
     * @return: An integer
     */
    public int ladderLength(String start, String end, Set<String> dict) {
        // write your code here
        
        Queue<String> queue = new LinkedList<>();
        Set<String> set = new HashSet<>();
        queue.offer(start);
        set.add(start);
        dict.add(end);
        
        int level = 0;
        while (!queue.isEmpty()) {
            int s = queue.size();
            level++;
            for (int i = 0; i < s; i++) {
                String cur = queue.poll();
                if (cur.equals(end)) return level;
                for (String str : dict) {
                    if (set.contains(str)) continue;
                    if (helper(cur, str)) {
                        queue.offer(str);
                        set.add(str);
                    }
                }
            }
        }
        return -1;
    }
    
    public boolean helper(String str1, String str2) {
        int count = 0;
        for (int i = 0; i < str1.length(); i++) {
            if (count > 1) return false;
            if (str1.charAt(i) != str2.charAt(i)) count++;
        }
        return count == 1;
    }
    
}
```

所以，就结果而论，反而是先找出所有possible的next word，并同时check在不在字典里比较快（在生成的同时进行check，略有优化），至于我想的第二种方法，在字典较大时就超时了。

自己看吧

[https://www.jiuzhang.com/solution/word-ladder/](https://www.jiuzhang.com/solution/word-ladder/)


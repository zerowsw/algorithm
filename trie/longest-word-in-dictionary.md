# Longest Word in Dictionary

## Description

Given a list of strings `words` representing an English Dictionary, find the longest word in `words` that can be built one character at a time by other words in `words`. If there is more than one possible answer, return the longest word with the smallest lexicographical order.If there is no answer, return the empty string.

**Example 1:**

```text
Input: 
words = ["w","wo","wor","worl", "world"]
Output: "world"
Explanation: 
The word "world" can be built one character at a time by "w", "wo", "wor", and "worl".
```

**Example 2:**

```text
Input: 
words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
Output: "apple"
Explanation: 
Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".
```

**Note:**

All the strings in the input will only contain lowercase letters.

The length of `words` will be in the range `[1, 1000]`.

The length of `words[i]` will be in the range `[1, 30]`.

## Solution

这道题虽然是Easy。。

非常typical的Trie的应用，搞得我连最基本的brute force的方法都不知道了（直接检查String的每个prefix呗）

下面是trie的解法：

```java
class Solution {
    private class TrieNode{
        Map<Character, TrieNode> map;
        int index;
        public TrieNode() {
            map = new HashMap<>();
            index = -1;
        }
    }
    
    private class Trie{
        TrieNode root;
        
        public Trie() {
            root = new TrieNode();
        }
        
        public void insert(String word, int index) {
            TrieNode cur = this.root;
            for (Character chr : word.toCharArray()) {
                cur = cur.map.computeIfAbsent(chr, k -> new TrieNode());
            }
            cur.index = index;
        }
    }
    
    public String longestWord(String[] words) {
        Trie tree = new Trie();
        for (int i = 0; i < words.length; i++) {
            tree.insert(words[i], i);
        }
        
        TrieNode root = tree.root;
        Deque<TrieNode> stack = new LinkedList<>();
        stack.push(root);
        String ans = "";
        while (!stack.isEmpty()) {
            TrieNode cur = stack.pop();
            for (TrieNode node : cur.map.values()) {
                if (node.index != -1) {
                    stack.push(node);
                }
            }
            if (cur.index == - 1) continue;
            String curword = words[cur.index];
            if ( curword.length() > ans.length() || curword.length() == ans.length() && curword.compareTo(ans) < 0) {
                ans = curword;
            }
        }
        return ans;
    }
}
```


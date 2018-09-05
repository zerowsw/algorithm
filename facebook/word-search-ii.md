---
description: 这题的解法非常的优雅！！！
---

# Word Search II

## Description

Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

## Example

```text
Input: 
words = ["oath","pea","eat","rain"] and board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

Output: ["eat","oath"]
```

**Note:**  
You may assume that all inputs are consist of lowercase letters `a-z`.

## Solution

想来也不是简单的用之前的word search来check每一个word。

用Trie做了优化，优化在哪儿呢？如果按照你的想法，扫描board的时候，对于每一个board中的字符，无论它是否存在于（主要是不存在的时候）list里面的word当中，我们对于每一个word都需要check一遍。如果用Trie来处理的话，那么只需要check一遍就可以了。

这道题也能很好地帮助我理解Trie的作用。

```java
class Solution {
    
    private class TrieNode{
        TrieNode[] next;
        String word;
        public TrieNode() {
            next = new TrieNode[26];
        } 
    }
    public List<String> findWords(char[][] board, String[] words) {
        TrieNode root = consturctTrie(words);
        List<String> res = new ArrayList<>();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                helper(res, board, i, j, root);
            }
        }
        return res;
    }
    
    public void helper(List<String> res, char[][] board, int x, int y, TrieNode root) {
        //end condition   
        char c = board[x][y];
        if (c == '#' || root.next[c - 'a'] == null) return;
        TrieNode next = root.next[c - 'a'];
        
         if (next.word != null) {
            res.add(next.word);
            next.word = null;
        }
        
        board[x][y] = '#';
        if (x > 0) helper(res, board, x - 1, y, root.next[c - 'a']);
        if (x < board.length - 1) helper(res, board, x + 1, y, root.next[c - 'a']);
        if (y > 0) helper(res, board, x, y - 1, root.next[c - 'a']);
        if (y < board[0].length - 1) helper(res, board, x, y + 1, root.next[c - 'a']);
        board[x][y] = c;
    }
    
    public TrieNode consturctTrie(String[] words) {
        TrieNode root = new TrieNode();
        for (String word : words) {
            TrieNode pointer = root;
            for (char cur : word.toCharArray()) {
                if (pointer.next[cur - 'a'] == null) pointer.next[cur - 'a'] = new TrieNode();
                pointer = pointer.next[cur - 'a'];
            }
            pointer.word = word;
        }
        return root;
    }
}
```


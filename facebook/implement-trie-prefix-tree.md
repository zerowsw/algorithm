---
description: 作为一种比较高级的经常使用的数据结构，必须要掌握！！！
---

# Implement Trie \(Prefix Tree\)

## Description

Implement a trie with `insert`, `search`, and `startsWith` methods.

## Example

```text
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

## Solution

其实，关于字典树的原理，我非常清楚，只不过很少写。

不考虑程序的鲁棒性，扩展性，只是单纯为了实现相关的功能的话，我的代码如下：

```java
class Trie {  
    private class TrieNode{
        TrieNode[] links;
        boolean isEnd;
        public TrieNode() {
            links = new TrieNode[26];
        }
    }
    
    TrieNode root;
    
    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            if (node.links[word.charAt(i) - 'a'] == null) {
                node.links[word.charAt(i) - 'a'] = new TrieNode();
            }
            node = node.links[word.charAt(i) - 'a'];
        }
        node.isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            if (node.links[word.charAt(i) - 'a'] == null) return false;
            node = node.links[word.charAt(i) - 'a'];
        }
        return node.isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode node = root;
        for (int i = 0; i < prefix.length(); i++) {
            if (node.links[prefix.charAt(i) - 'a'] == null) return false;
            node = node.links[prefix.charAt(i) - 'a'];
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```




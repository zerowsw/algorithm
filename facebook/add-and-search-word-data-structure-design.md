# Add and Search Word - Data structure design

## Description

Design a data structure that supports the following two operations:

```text
void addWord(word)
bool search(word)
```

search\(word\) can search a literal word or a regular expression string containing only letters `a-z` or `.`. A `.` means it can represent any one letter.

## Example

```text
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

## Solution

直接来看的话，首先会考虑使用Set来存储word。问题的关键在于如何处理这个正则。

想法一，用把  .   换成其他字母，找到所有可能的String， 然后再进行查找。

想法二， 坑爹了，真的要用字典树？那样的话，我觉得就是结合HashSet和字典树，更有效率。正好通过这道题，搞一搞trie的写法。

先做，implement trie吧

```java
class WordDictionary {
    
    private class TrieNode{
        TrieNode[] links;
        boolean isEnd;
        public TrieNode() {
            links = new TrieNode[26];
        }
    }
    
    TrieNode root;
    
    /** Initialize your data structure here. */
    public WordDictionary() {
        root = new TrieNode();
    }
    
    /** Adds a word into the data structure. */
    public void addWord(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            if (node.links[word.charAt(i) - 'a'] == null) {
                node.links[word.charAt(i) - 'a'] = new TrieNode();
            }
            node = node.links[word.charAt(i) - 'a'];
        }
        node.isEnd = true;
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    public boolean search(String word) {
        TrieNode node = root;
        return helper(node, word);
    }
    
    private boolean helper(TrieNode node, String word) {
        if (node == null) return false;
        for (int i = 0; i < word.length(); i++) {
            if (word.charAt(i) == '.') {
                for (int j = 0; j < 26; j++) {
                    if (helper(node.links[j], word.substring(i + 1, word.length()))) {
                        return true;
                    }
                }
                return false;
            }
            if (node.links[word.charAt(i) - 'a'] == null) return false;
            node = node.links[word.charAt(i) - 'a'];
        }
        return node.isEnd;
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```


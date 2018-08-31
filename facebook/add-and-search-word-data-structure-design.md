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


# Palindrome Pairs

## Description

Given a list of **unique** words, find all pairs of **distinct** indices `(i, j)` in the given list, so that the concatenation of the two words, i.e. `words[i] + words[j]` is a palindrome.

## Example

**Example 1:**

```text
Input: ["abcd","dcba","lls","s","sssll"]
Output: [[0,1],[1,0],[3,2],[2,4]] 
Explanation: The palindromes are ["dcbaabcd","abcddcba","slls","llssssll"]
```

**Example 2:**

```text
Input: ["bat","tab","cat"]
Output: [[0,1],[1,0]] 
Explanation: The palindromes are ["battab","tabbat"]
```

## Solution

首先一个brute force的方法就是找出所有的pair, 然后，再判断是不是palindrome

我刚才在思考怎么样进行优化，理论上判断palindrome用两个指针就可以解决了，那么优化的方向就在于，如何减少比较的pair的数量。 Trie就是一个比较好的选择。

这个后面再复习吧，现在先过掉（主要是，实在懒得写了）

[https://www.jiuzhang.com/solution/palindrome-pairs/](https://www.jiuzhang.com/solution/palindrome-pairs/)

[https://leetcode.com/problems/palindrome-pairs/discuss/79195/O\(n\*k2\)-java-solution-with-Trie-structure](https://leetcode.com/problems/palindrome-pairs/discuss/79195/O%28n*k2%29-java-solution-with-Trie-structure)

[https://leetcode.com/problems/palindrome-pairs/discuss/79199/150-ms-45-lines-JAVA-solution](https://leetcode.com/problems/palindrome-pairs/discuss/79199/150-ms-45-lines-JAVA-solution)



卧槽，，，你他么。。。老子还等着看你的解释呢。。


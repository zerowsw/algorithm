# Palindromic Substrings

## Description

Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

## Example

**Example 1:**  


```text
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

**Example 2:**  


```text
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

## Solution

N^ 2的从中心扩展的方法就直接跳过了。。

主要看看Manacher’s Algorithm, 马拉车算法，妈蛋，感觉不会考：

{% embed data="{\"url\":\"https://blog.csdn.net/liuwei0604/article/details/50414542\",\"type\":\"link\",\"title\":\"最长回文字符串算法-Manacher’s  Algorithm-马拉车算法 - CSDN博客\",\"description\":\"本文翻译于LeetCode上关于最长回文字符串的说明  除了翻译之外，其中还加入了个人的理解的部分，将其中没有详细说明的部分进行了解释。  时间复杂度为O\(n\)的算法  首先，我们需要讲输入的字符串 S 进行一下转换得到 T，转换的方法就是通过在每两个字符之间插入一个字符串“\#”,你马上就能知道为什么要这么做。  例如 输入字符串 S = “abaaba”, 转换之后得到了 T = “\#a\#b\#a\",\"icon\":{\"type\":\"icon\",\"url\":\"https://csdnimg.cn/public/favicon.ico\",\"aspectRatio\":0}}" %}




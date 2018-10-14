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

{% embed url="https://blog.csdn.net/liuwei0604/article/details/50414542" %}




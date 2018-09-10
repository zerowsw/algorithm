# Minimum Size Subarray Sum

## Description

Given an array of **n** positive integers and a positive integer **s**, find the minimal length of a **contiguous** subarray of which the sum ≥ **s**. If there isn't one, return 0 instead.

## Example

```text
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```

## Follow up

If you have figured out the O\(n\) solution, try coding another solution of which the time complexity is O\(n log n\). 

## Solution

首先想到的是，prefix sum。。根据当前的最窗口长度，可以进行一定优化。。

然后一想，这部也可以用sliding window来做嘛。。卧槽，关于sliding window这块我还是想错了。。

不要因为上一道题是固定长度的，这道题就思维定势了。。

此外，看清题目，这道题所有的element都是positive的。。

好吧，仔细想想，怎么做都可以。。

妈蛋，sliding window不写了。。

看看binary search的。。，其实没啥特别的，就是用prefix sum的时候，第二层存换使用binary search。

此外，这里学到的非常重要的一点就是，用prefix sum的时候，不要怕麻烦，比较好的方式是先用一个O\(n\)的时间，存下prefix sum数组的值。






---
description: 这题不就是Sai给我出的第二个面试题嘛，就是不知道要不要考虑越界的情况
---

# Search in a Big Sorted Array

## Description

Given a big sorted array with positive integers sorted by ascending order. The array is so big so that you can not get the length of the whole array directly, and you can only access the kth number by `ArrayReader.get(k)` \(or ArrayReader-&gt;get\(k\) for C++\). Find the first index of a target number. Your algorithm should be in O\(log k\), where k is the first index of the target number.

Return -1, if the number doesn't exist in the array.

## Example

Given `[1, 3, 6, 9, 21, ...]`, and target = `3`, return `1`.

Given `[1, 3, 6, 9, 21, ...]`, and target = `4`, return `-1`.

## Challenge

O\(log k\), k is the first index of the given target number.

## Solution

基本思路还是很明了的，不过这题不需要考虑越界的情况。

在做这题的时候，我没有仔细审题，数组中的元素是有重复的，我们需要找目标元素的第一个下标。

再次感觉我这样用一个默认值为-1的变量存储查找结果是一个十分机智的做法。

```java
public class Solution {
    /*
     * @param reader: An instance of ArrayReader.
     * @param target: An integer
     * @return: An integer which is the first index of target.
     */
    public int searchBigSortedArray(ArrayReader reader, int target) {
        // write your code here
        int index = 1;
        while (reader.get(index) < target) {
            index *= 2;
        }
        int le = 0;
        int ri = index;
        int res = -1;
        while (le <= ri) {
            int mid = le + (ri - le) / 2;
            if (reader.get(mid) == target) {
                res = mid;
                ri = mid - 1;
            } else if (reader.get(mid) > target) {
                ri = mid - 1;
            } else {
                le = mid + 1;
            }
        }
        return res;
    }
}
```


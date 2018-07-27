# Merge K Sorted Arrays

## Description

Given _k_ sorted integer arrays, merge them into one sorted array.

## Example

Given 3 sorted arrays:

```text
[
  [1, 3, 5, 7],
  [2, 4, 6],
  [0, 8, 9, 10, 11]
]
```

return `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]`.

## Challenge

Do it in O\(N log k\).

* _N_ is the total number of integers.
* _k_ is the number of arrays.

## Solution

没啥好说的，老套路。 为了能够获取下一个节点，可以参照Merge K Sorted Interval的方法，定义包含Row 和Column信息的Element \(也可以直接就把value放进去\)。

不写了。


# Maximum Distance in Arrays

## Description

Given `m` arrays, and each array is sorted in ascending order. Now you can pick up two integers from two different arrays \(each array picks one\) and calculate the distance. We define the distance between two integers `a` and `b` to be their absolute difference `|a-b|`. Your task is to find the maximum distance.

**Example 1:**  


```text
Input: 
[[1,2,3],
 [4,5],
 [1,2,3]]
Output: 4
Explanation: 
One way to reach the maximum distance 4 is to pick 1 in the first or third array and pick 5 in the second array.
```

**Note:**

1. Each given array will have at least 1 number. There will be at least two non-empty arrays.
2. The total number of the integers in **all** the `m` arrays will be in the range of \[2, 10000\].
3. The integers in the `m` arrays will be in the range of \[-10000, 10000\].

## Solution

虽然只是道easy，但我真没有想到这种简洁的做法，有点DP的感觉，但其实又不是。。

```java
class Solution {
    public int maxDistance(List<List<Integer>> arrays) {
        int max = arrays.get(0).get(arrays.get(0).size() - 1);
        int min = arrays.get(0).get(0);
        
        int res = Integer.MIN_VALUE;
        
        for (int i = 1; i < arrays.size(); i++) {
            List<Integer> array = arrays.get(i);
            res = Math.max(res, array.get(array.size() - 1) - min);
            res = Math.max(res, max - array.get(0));
            max = Math.max(array.get(array.size() - 1), max);
            min = Math.min(array.get(0), min);
        }
        return res;
    }
}
```


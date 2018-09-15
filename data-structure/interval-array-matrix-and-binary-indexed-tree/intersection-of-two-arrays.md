# Intersection of Two Arrays

## Description

Given two arrays, write a function to compute their intersection.

## Example

Given _nums1_ = `[1, 2, 2, 1]`, _nums2_ = `[2, 2]`, return `[2]`.

## Challenge

Can you implement it in _three_ different algorithms?

## Solution

有兴趣再看看别的解法吧：[https://www.jiuzhang.com/solution/intersection-of-two-arrays/](https://www.jiuzhang.com/solution/intersection-of-two-arrays/)

但两者的size大小关系成为一个比较重要的考虑因素：[https://www.geeksforgeeks.org/union-and-intersection-of-two-sorted-arrays-2/](https://www.geeksforgeeks.org/union-and-intersection-of-two-sorted-arrays-2/)， 尼玛，居然真的使用binary search进行优化的。。



```java
public class Solution {
    
    /*
     * @param nums1: an integer array
     * @param nums2: an integer array
     * @return: an integer array
     */
    public int[] intersection(int[] nums1, int[] nums2) {
        // write your code here
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> set2 = new HashSet<>();
        
        for (Integer inte : nums1) {
            set1.add(inte);
        }
        for (Integer inte : nums2) {
            if (set1.contains(inte)) {
                set2.add(inte);
            }
        }
        int[] res = new int[set2.size()];
        int index = 0;
        for (Integer inte : set2) {
            res[index++] = inte;
        }
        return res;
    }
};
```


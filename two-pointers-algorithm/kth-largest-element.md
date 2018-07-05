---
description: 经典题，纯粹练练手
---

# Kth Largest Element

## Description

Find K-th largest element in an array.

## Solution

面试的时候也碰到过，经典中的经典，需要秒解的那种。基本上能答出三种方法应该就能过关了。

1. brute force的方法，排序，然后，一切就都好说了。
2. 用一个固定大小的heap来处理。
3. QuickSelect算法。

妈蛋，边界条件崩掉了。。一不小心多了个=就overflow了

```java
class Solution {
    /*
     * @param k : description of k
     * @param nums : array of nums
     * @return: description of return
     */
    public int kthLargestElement(int k, int[] nums) {
        // write your code here
        //练一练quick select吧
        return quickselect(nums, 0, nums.length - 1, k);
    }
    
    public int quickselect(int[] nums, int left, int right, int k) {
        //end condition
        if (left >= right) return nums[left];
        
        int le = left;
        int ri = right;
        int v = nums[le];
        while (le <= ri) {
            while (le <= ri && nums[le] > v) {
                le++;
            }
            while (le <= ri && nums[ri] < v) {
                ri--;
            }
            if (le <= ri) {
                int temp = nums[ri];
                nums[ri] = nums[le];
                nums[le] = temp;
                le++;
                ri--;
            }
        }
        if (left + k - 1 >= le) {
            return quickselect(nums, le, right, k - (le - left));
        }
        if (left + k - 1 <= ri) {
            return quickselect(nums, left, ri, k);
        }
        return nums[ri + 1];
    }
};
```


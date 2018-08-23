# Next Permutation

## Description

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order \(ie, sorted in ascending order\).

The replacement must be [**in-place**](http://en.wikipedia.org/wiki/In-place_algorithm) and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

`1,2,3` → `1,3,2`  
`3,2,1` → `1,2,3`  
`1,1,5` → `1,5,1`

## Solution

解决这道题的关键在于如何将调整permutation的顺序反映到数组元素的大小关系上，显而易见的是，单调递增的序列就是初始序列，单调递减的序列就是最终的序列。

要想找到下一个permutation, 数字的调整肯定是从右侧找起，找到第一个递增序列，然后就需要找到怎么样进行调整了。其实考虑到是要找next permutation，前半部分是完全不需要动的（你自己想一想）。然后自你找到的第一个递增自序列起，是可以保证找到一个大一些的排列方式的。

交换递增子序列紧邻山顶元素和右侧递减序列中刚好大于它的元素，然后元素按照递增顺序排列。

所以，其实这道题，代码挺简单的。。。。。

注意，递减序列的next permutation是递增序列

```java
class Solution {
    public void nextPermutation(int[] nums) {
        
        if (nums == null || nums.length <= 1) return;
        
        int target = nums.length - 1;
        while (target > 0 && nums[target] <= nums[target - 1] ) {
            target--;
        }
        
        if (target > 0) {
            int a = target - 1;
            int b = target;
            while (b < nums.length - 1 && nums[b + 1] > nums[a]) {
                b++;
            }
            swap(nums, a, b);
        }
        int le = target;
        int ri = nums.length - 1;
        while (le < ri) {
            swap(nums, le++, ri--);
        }
    }
    
    private void swap(int[] nums, int a, int b) {
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}
```


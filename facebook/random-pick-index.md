# Random Pick Index

## Description

Given an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.

**Note:**  
The array size can be very large. Solution that uses too much extra space will not pass the judge.

## Example

```text
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(3);

// pick(1) should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(1);
```

## Solution

拿到这道题之后，我脑中的想法是：如果允许额外的空间的话，我可以用一个HashMap存储一下。如果允许的话，就先排序，然后用二分搜索找到起始下标，然后用水库抽样。

看了下topic，果然有reservoir sampling。。

O\(n\)的时间Pick

```java
class Solution {

    int[] nums;
    Random rand;
    public Solution(int[] nums) {
        this.nums = nums;
        rand = new Random();
    }
    
    public int pick(int target) {
       int res = -1;
       int count = 0;
       for (int i = 0; i < nums.length; i++) {
           if (nums[i] != target) continue;
           if (rand.nextInt(++count) == 0) res = i;
       }
       return res;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int param_1 = obj.pick(target);
 */
```


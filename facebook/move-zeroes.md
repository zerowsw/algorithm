# Move Zeroes

## Description

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

## Solution

你是不是脑残，居然又被corner卡住了。。注意啊！！！

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int countOfZ = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                countOfZ++;
            } else if (countOfZ == 0) {
                continue;
            } else {
                nums[i - countOfZ] = nums[i];
                nums[i] = 0;
            }
        }
    }
}
```


# Product of Array Except Self

## Description

Given an array `nums` of _n_ integers where _n_ &gt; 1,  return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

## Example

```text
Input:  [1,2,3,4]
Output: [24,12,8,6]
```

## Note

Please solve it **without division** and in O\(_n_\).

## Follow up

Could you solve it with constant space complexity? \(The output array **does not** count as extra space for the purpose of space complexity analysis.\)

## Solution

做过，没啥好说的。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] res = new int[nums.length];
        Arrays.fill(res, 1);
        int product = 1;
        for (int i = 0; i < nums.length; i++) {
            res[i] *= product;
            product *= nums[i];
        }
        product = 1;
        for (int i = nums.length - 1; i >= 0; i--) {
            res[i] *= product;
            product *= nums[i];
        }
        return res;
    }
}
```


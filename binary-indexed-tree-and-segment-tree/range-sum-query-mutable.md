# Range Sum Query - Mutable

## Description

Given an integer array nums, find the sum of the elements between indices i and j \(i ≤ j\), inclusive.

The update\(i, val\) function modifies nums by updating the element at index i to val.

**Example:**

```text
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```

**Note:**

1. The array is only modifiable by the update function.
2. You may assume the number of calls to update and sumRange function is distributed evenly.

## Solution

相当直接的FenWickTree或者说Binary Indexed Tree的应用。

也是我第一次手撸的Binary Indexed Tree, 其实真的相当简单：

```java
class NumArray {
    
    int[] fenwicktree;
    int[] numbers;

    public NumArray(int[] nums) {
        fenwicktree = new int[nums.length + 1];
        this.numbers = new int[nums.length];
        for (int i =  0; i < nums.length; i++) {
            update(i, nums[i]);
        }
    }
    
    public void update(int i, int val) {
        int delta = val - numbers[i];
        numbers[i] = val;
        int index = i + 1;
        while (index < fenwicktree.length) {
            fenwicktree[index] += delta;
            index += lowBit(index);
        }
    }
    
    public int sum(int i) {
        int sum = 0;
        while (i > 0) {
            sum += fenwicktree[i];
            i -= lowBit(i);
        }
        return sum;
    }
    
    private int lowBit(int x) {
        return x & (-x);
    }
    
    public int sumRange(int i, int j) {
        return sum(j + 1) - sum(i);
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```


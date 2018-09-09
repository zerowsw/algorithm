# Sort Transformed Array

## Description

Given a **sorted** array of integers nums and integer values a, b and c. Apply a quadratic function of the form f\(x\) = ax2 + bx + c to each element x in the array.

The returned array must be in **sorted order**.

Expected time complexity: **O\(n\)**

## Example

**Example 1:**

```text
Input: nums = [-4,-2,2,4], a = 1, b = 3, c = 5
Output: [3,9,15,33]
```

**Example 2:**

```text
Input: nums = [-4,-2,2,4], a = -1, b = 3, c = 5
Output: [-23,-5,1,7]
```

## Solution

好像做过，可以结合二次方程的性质，比如对称轴啥的。

我去，傻的嘛，谁让你in-place了，就是找到中心位置，用两个指针向外移就可以了，存到结果数组里。唉，又想跳过了。。

找中心位置的话，用binary search，或者干脆直接扫吧，反正时间复杂度也是O\(n\)

额，不对，妈勒个鸡，不用从中间往两边扩散，从两边往中间处理就好了，相当于先找最大值。

没有注意根据a的正负可以分成凸函数或者凹函数，需要分类写，我的写法看起来比较繁琐：

```java
class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        int le = 0;
        int ri = nums.length - 1;
        int[] res = new int[nums.length];
        
        for (int i = nums.length - 1; i >= 0; i--) {
            int left = getvalue(nums[le], a, b, c) ;
            int right = getvalue(nums[ri], a, b, c);
            if (a > 0) {
            res[i] = left < right ? getvalue(nums[ri--], a, b, c) : getvalue(nums[le++], a, b, c);
            } else {
            res[nums.length - 1 - i] = left > right ?  getvalue(nums[ri--], a, b, c) : getvalue(nums[le++], a, b, c);
            }
        }
        return res;
    }
    public int getvalue(int v, int a, int b, int c) {
        return a * v * v + b * v + c;
    }
}
```

看来下，高票答案，没啥区别，也是这样写得，只不过，没有额外定义left，right，是直接算的。


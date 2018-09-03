---
description: 经典题
---

# Trapping Rain Water

## Description

Given _n_ non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

![](http://www.leetcode.com/static/images/problemset/rainwatertrap.png)  
The above elevation map is represented by array \[0,1,0,2,1,0,1,3,2,1,2,1\]. In this case, 6 units of rain water \(blue section\) are being trapped. **Thanks Marcos**for contributing this image!

## Example

```text
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

## Solution

经典题，甚至剑指offer上都有。

我目前记得的比较简单直白的方法就是左右各扫一遍。（反而忘了怎么搞）

只记得两根指针同时扫了（同时也是最有解）：

```java
class Solution {
    public int trap(int[] height) {
        
        //corner case
        if (height == null || height.length == 0) return 0;
        int le = 0;
        int ri = height.length - 1;
        int lh = height[le];
        int rh = height[ri];
        int water = 0;
        while (le < ri) {
            while (le < ri && lh <= rh) {
                if (lh < height[le]) {
                    lh = height[le];
                    continue;
                }
                water += (lh - height[le++]);
            }
            while (le < ri && rh <= lh) {
                if (rh < height[ri]) {
                    rh = height[ri];
                    continue;
                }
                water += (rh - height[ri--]);
            }
        }
        return water;
    }
}
```

补充一下，扫两边的方法，就是一种Dynamic Programming的思想，也就是说，左扫一遍，记录一下从左往右的height的最大值，然后右扫一遍，记录下从右往左的height的最大值。 然后在每个位置，有左右最大值的较小着来算水量。


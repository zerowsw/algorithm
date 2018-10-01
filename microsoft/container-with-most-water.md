# Container With Most Water

## Description

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate \(i, ai\). n vertical lines are drawn such that the two endpoints of line i is at \(i, ai\) and \(i, 0\). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and n is at least 2.

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array \[1,8,6,2,5,4,8,3,7\]. In this case, the max area of water \(blue section\) the container can contain is 49.

**Example:**

```text
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

## Solution

这个么，直奔最优解而去。两个指针。

不过一开始做错了，我用了下一个位置的高度来判断指针的移动。

应该使用当前位置的高度来。。你好好想想：

```java
class Solution {
    public int maxArea(int[] height) {
        if (height == null || height.length <= 1) return 0; 
        int le = 0;
        int ri = height.length - 1;
        int max = Integer.MIN_VALUE;
        while (le < ri) {
            max = Math.max(max, (ri - le) * Math.min(height[le], height[ri]));
            if (height[ri] < height[le]) {
                ri--;
            } else {
                le++;
            }
        }
        return max;
    }
}
```


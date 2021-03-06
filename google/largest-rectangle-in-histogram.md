# Largest Rectangle in Histogram

## Description

Given _n_ non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![](https://leetcode.com/static/images/problemset/histogram.png)  
Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.

![](https://leetcode.com/static/images/problemset/histogram_area.png)  
The largest rectangle is shown in the shaded area, which has area = `10` unit.

## Example

**Example:**

```text
Input: [2,1,5,6,2,3]
Output: 10
```

## Solution

要好好理解一下这个单调栈的逻辑：

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        Deque<Integer> stack = new LinkedList<>();
        stack.push(-1);
        int maxArea = Integer.MIN_VALUE;
        for (int i = 0; i < heights.length; i++) {
            while (stack.peek() != -1 && heights[i] < heights[stack.peek()]) {
                maxArea = Math.max(maxArea, heights[stack.pop()] * (i - stack.peek() - 1));
            }
            stack.push(i);
        
        }
        while (stack.peek() != -1) {
            maxArea = Math.max(maxArea, heights[stack.pop()] * (heights.length - stack.peek() - 1));
        }
        return maxArea == Integer.MIN_VALUE ? 0 : maxArea;
    }
}
```


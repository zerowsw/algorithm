# Sliding Window Maximum

## Description

Given an array _nums_, there is a sliding window of size _k_ which is moving from the very left of the array to the very right. You can only see the _k_ numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

**Example:**

```text
Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Note:**   
You may assume _k_ is always valid, 1 ≤ k ≤ input array's size for non-empty array.

**Follow up:**  
Could you solve it in linear time?

## Solution

妈勒个鸡，惊觉我连这道题都不会做。。

不知之前的sliding window的方法，我们需要记录一下这个K size的窗口里面的元素，这样在离开某个位置的元素的时候我们才能知道下一个最大值是啥。

用heap是一个选择，不过在删除元素的时候，时间复杂度不是很好。

然后我想到用deque来做,  好吧，这道题用的不是什么单调栈，而是单调队列。。

感觉自己的脑子整天都是浑浑噩噩的啊。。远远比不上几年前的自己了。。

记住一个情况，在K size的window范围内，如果有num\[x\] &lt; num\[i\], 且x &lt; i, 那么num\[x\]永远没有机会成为成为接下来的window当中的最大值。

我还在想，不记录下标，只记录值也能用queue的size来判断需不需要poll队首元素，实在是naive，你还真的需要记录下标。。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length == 0) return new int[]{};
        Deque<Integer> queue = new ArrayDeque<>();
        int le = 0;
        int ri = 0;
        int[] res = new int[nums.length - k + 1];
        int index = 0;
        while (ri < nums.length) {
            if (ri - le < k) {
                while (!queue.isEmpty() && nums[queue.peekLast()] < nums[ri]) {
                    queue.pollLast();
                }
                queue.offer(ri++);
            }
            if (ri - le == k) res[index++] = nums[queue.peek()];
            if (ri - le >= k && queue.peek() == le++) {
                queue.poll();
            }
        }
        return res;
    }
}
```




# Next Greater Element I

## Description

You are given two arrays **\(without duplicates\)** `nums1` and `nums2` where `nums1`’s elements are subset of `nums2`. Find all the next greater numbers for `nums1`'s elements in the corresponding places of `nums2`.

The Next Greater Number of a number **x** in `nums1` is the first greater number to its right in `nums2`. If it does not exist, output -1 for this number.

**Example 1:**

```text
Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.
```

**Example 2:**

```text
Input: nums1 = [2,4], nums2 = [1,2,3,4].
Output: [3,-1]
Explanation:
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.
```

**Note:**

1. All elements in `nums1` and `nums2` are unique.
2. The length of both `nums1` and `nums2` would not exceed 1000.

## Solution

怎么感觉不是那么好做呢。。

这种题目，首先嘛，确实可以用Heap来做，时间复杂度为O\(nlogk\)。

然后，我觉得也可以用单调栈来做。。 时间复杂度为O\(m + n\)

OK，最优解真的就是用单调栈（Monotonous stack）来做的。

我的解法：

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Deque<Integer> stack = new LinkedList<>();
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = nums2.length - 1; i >= 0; i--) {
            if (stack.isEmpty()) {
                map.put(nums2[i], -1);
            } else {
                while (!stack.isEmpty() && stack.peek() < nums2[i]) {
                    stack.pop();
                }
                if (!stack.isEmpty()) {
                    map.put(nums2[i], stack.peek());
                } else {
                    map.put(nums2[i], -1);
                }
            }
            stack.push(nums2[i]);
        }
        for (int i = 0; i < nums1.length; i++) {
            nums1[i] = map.get(nums1[i]);
        }
        return nums1;
    }
}
```

可以优化的情况，就是不存-1的情况，最后用getOrDefault\(i,  -1\)来填，主要是就是逻辑代码稍微写得少点。


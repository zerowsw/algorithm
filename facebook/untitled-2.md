# Target Sum

## Description

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+` and `-`. For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.



**Example 1:**  


```text
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

**Note:**  


1. The length of the given array is positive and will not exceed 20.
2. The sum of elements in the given array will not exceed 1000.
3. Your output answer is guaranteed to be fitted in a 32-bit integer.

## Solution

我日，除了DFS，还有啥，难道DP？

恭喜你答对了，显然是这样的。。

直接想DP好像有点问题，试试DFS with Memory Cache吧。。

这是我自己写的DFS  with Cache: 

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        return helper(nums, S, 0, new HashMap<>());
    }
    
    public int helper(int[] nums, int target, int index, Map<String, Integer> cache) {
        if (cache.containsKey(index + "_" + target)) {
            return cache.get(index + "_" + target);
        }
        if (index >= nums.length) {
            if (target == 0) {
                cache.put(index + "_" + target, 1);
                return 1;
            }
            return 0;
        }
        
        int add = helper(nums, target - nums[index], index + 1, cache);
        int minus = helper(nums, target + nums[index], index + 1, cache);
        cache.put(index + "_" + target, add + minus);
        return add + minus;
    }
}
```

然后，关于时空复杂度的分析：（分析的方法或者说角度，很值得学习）

* Time complexity : O\(l\*n\)O\(l∗n\). The memomemo array of size l\*nl∗n has been filled just once. Here, ll refers to the range of sumsum and nn refers to the size of numsnums array.
* Space complexity : O\(n\)O\(n\). The depth of recursion tree can go upto nn.

另外，其实当你想到了DFS with memoization的时候，DP的做法也很近了。。（思维只需要一个小小的跃进），这里不写了。


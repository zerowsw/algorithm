# Longest Consecutive Sequence

## Description

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Your algorithm should run in O\(_n_\) complexity.

## Example

```text
Input: [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

## Solution

灵光一闪，想了个解法，用HashMap 扫两次，但是没有考虑到有重复元素的情况：

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int inte = nums[i];
            if (map.containsKey(inte - 1)) {
                map.put(inte, map.get(inte - 1) + 1);
            } else if (!map.containsKey(inte)){
                map.put(inte, 1);
            }
        }
        
        for (int i = nums.length - 1; i >= 0; i--) {
            int inte = nums[i];
             if (map.containsKey(inte - 1)) {
                map.put(inte, map.get(inte - 1) + 1);
             } 
        }
        
        int max = 0;
        for (int val : map.values()) {
            max = Math.max(max, val);
        }
        return max;
    }
}
```

比如【0，0，1，-1】 就跪了。。

扫两遍并不能解决这个问题。

其实也可以构建链表，但时间复杂度会略微超过O\(n\)

对了，那我同时记录前面有多少个呢。。

我错了，还是不行。。

看了下解答，什么brute force + HashSet的 O\(n ^ 2\)的方法， 或者先sort，再扫描的O\(nlogn\)的方法都没啥意思。。

我日了够了，原来这个简单。。

brute force的基础上用HashSet进行优化， 然后只有再遇到sequence头部的时候才去尝试构建最长element  sequence。这样其实就自然而然地解决了重复扫描的问题。

其实思路相当简单明了。

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (Integer inte : nums) {
            set.add(inte);
        }
        
        int maxlen = 0;
        for (Integer num : nums) {
            if (!set.contains(num - 1)) {
                int cur = num;
                int curlen = 1;
                while (set.contains(cur + 1)) {
                    cur += 1;
                    curlen += 1;
                }
                maxlen = Math.max(maxlen, curlen);
            }
        }
        return maxlen;
    }
}
```


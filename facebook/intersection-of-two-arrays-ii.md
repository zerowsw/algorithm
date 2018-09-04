---
description: 这道题要好好看看follow up!!!
---

# Intersection of Two Arrays II

## Description

Given two arrays, write a function to compute their intersection.

**Example 1:**

```text
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```

**Example 2:**

```text
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```

**Note:**

* Each element in the result should appear as many times as it shows in both arrays.
* The result can be in any order.

## Follow Up

* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to nums2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

## Solution

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int v : nums1) {
            map.put(v, map.getOrDefault(v, 0) + 1);
        }
        List<Integer> res = new ArrayList<>();
        for (int v : nums2) {
            if (map.containsKey(v) && map.get(v) > 0) {
                res.add(v);
                map.put(v, map.get(v) - 1);
            }
        }
        int[] r = new int[res.size()];
        for (int i = 0; i < res.size(); i++) {
            r[i] = res.get(i);
        }
        return r;
    }
}
```


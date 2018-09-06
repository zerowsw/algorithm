# Increasing Subsequences

## Description

Given an integer array, your task is to find all the different possible increasing subsequences of the given array, and the length of an increasing subsequence should be at least 2 .

## Example

**Example:**  


```text
Input: [4, 6, 7, 7]
Output: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
```

**Note:**  


1. The length of the given array will not exceed 15.
2. The range of integer in the given array is \[-100,100\].
3. The given array may contain duplicates, and two equal integers should also be considered as a special case of increasing sequence.

## Solution

较为明显的DFS

想了想去重的方法，决定用Set来搞：

```java
class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        helper(res, nums, 0, new ArrayList<>());
        return res;
    }
    
    public void helper(List<List<Integer>> res, int[] nums, int index, List<Integer> cur) {
        if (cur.size() > 1) {
            res.add(new ArrayList<>(cur));
        }
        if (index >= nums.length) {
            return;
        }  
        
        Set<Integer> used = new HashSet<>();
        for (int i = index; i < nums.length; i++) {
            if (used.contains(nums[i])) continue;
            
            if (cur.size() == 0 || nums[i] >= cur.get(cur.size() - 1)) {
                cur.add(nums[i]);
                helper(res, nums, i + 1, cur);
                cur.remove(cur.size() - 1);
                used.add(nums[i]);
            }
        }
    }
}
```

另外，也有人用Set&lt;List&lt;Integer&gt;&gt;来去重，感觉不够优雅，不苟同。


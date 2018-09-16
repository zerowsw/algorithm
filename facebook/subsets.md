# Subsets

## Description

Given a set of **distinct** integers, _nums_, return all possible subsets \(the power set\).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```text
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

## Solution

妈勒个鸡，我卡住了。。我一开始的代码是这样的：

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null) return result;
        helper(result, nums, 0, new ArrayList<>());
        return result;
    }
    
    private void helper(List<List<Integer>> result, int[] nums, int index, List<Integer> cur) {
        if (index >= nums.length) {
            result.add(new ArrayList<>(cur));
            return;
        }
        
        for (int i = index; i < nums.length; i++) {
            //helper(result, nums, i + 1, cur);
            cur.add(nums[i]);
            helper(result, nums, i + 1, cur);
            cur.remove(cur.size() - 1);   
        }
    }
}
```

傻了傻了，为啥非要到底才收呢。。

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null) return result;
        helper(result, nums, 0, new ArrayList<>());
        return result;
    }
    
    private void helper(List<List<Integer>> result, int[] nums, int index, List<Integer> cur) {
        result.add(new ArrayList<>(cur));
        for (int i = index; i < nums.length; i++) {
            cur.add(nums[i]);
            helper(result, nums, i + 1, cur);
            cur.remove(cur.size() - 1);   
        }
    }
}
```


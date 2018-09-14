# Path Sum III

## Description

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards \(traveling only from parent nodes to child nodes\).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

## Example

```text
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

## Solution

妈勒个鸡，好像做过Path Sum II

感觉并不好做啊。。

感觉可以自顶向下，在每个点都 check一下，虽然感觉时间复杂度好高哦。。哦，不对，你又忘了，prefix sum的问题是可以用hashmap, 或则说hashset来优化（不对，考虑到重复性的问题，这里还是要用hashmap）

我的用了全局变量的方法：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int pathSum(TreeNode root, int sum) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        helper(root, map, 0, sum);
        return count;
        
    }
    int count = 0;
    private void helper(TreeNode root, Map<Integer, Integer> map, int cur, int target) {
        if (root == null) return;
        cur += root.val;
        count += map.getOrDefault(cur - target, 0);
        map.put(cur, map.getOrDefault(cur, 0) + 1);
        helper(root.left, map, cur, target);
        helper(root.right, map, cur, target);
        map.put(cur, map.get(cur) - 1);
    }
}
```

不用全局变量的版本，其实就是自顶向下，和自底向上的区别：（差不多）

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int pathSum(TreeNode root, int sum) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        return helper(root, map, 0, sum);        
    }
    
    private int helper(TreeNode root, Map<Integer, Integer> map, int cur, int target) {
        if (root == null) return 0;
        cur += root.val;
        int count = map.getOrDefault(cur - target, 0);
        map.put(cur, map.getOrDefault(cur, 0) + 1);
        count += helper(root.left, map, cur, target);
        count += helper(root.right, map, cur, target);
        map.put(cur, map.get(cur) - 1);
        return count;
    }
}
```




# Binary Tree Right Side View

## Description

Given a binary tree, imagine yourself standing on the _right_ side of it, return the values of the nodes you can see ordered from top to bottom.

## Example

```text
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

## Solution

我觉得就是BFS，然后存储每层的最后一个，是的话，我就跳过这题了。

答案里的BFS写了一堆，不忍直视，这是我的：

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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                if (i == size - 1) result.add(cur.val);
                if (cur.left != null) queue.offer(cur.left);
                if (cur.right != null) queue.offer(cur.right);
            }
        }
        return result;
    }
}
```

看了下他的DFS的解法，核心思想就是每次先访问右子树，同时记录层数来判断需要正确存储的点，他是用的非递归做的，同样写得比较繁琐。

discuss 里面的解法第一条，用的递归的DFS解法，看起来就清爽多了，写一下：\(其实可以用res.size\(\) 代替max\_depth\)

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
    int max_depth = -1;
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        helper(root, result, 0);
        return result;
    }
    
    public void helper(TreeNode root, List<Integer> res, int depth) {
        if (root == null) return;
        if (depth > max_depth) {
            res.add(root.val);
            max_depth = depth;
        }
        helper(root.right, res, depth + 1);
        helper(root.left, res, depth + 1);
    }
}
```


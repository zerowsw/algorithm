# Binary Tree Vertical Order Traversal

## Description

Given a binary tree, return the vertical order traversal of its nodes' values. \(ie, from top to bottom, column by column\).

If two nodes are in the same row and column, the order should be from **left to right**.

## Example

**Examples 1:**

```text
Input: [3,9,20,null,null,15,7]

   3
  /\
 /  \
 9  20
    /\
   /  \
  15   7 

Output:

[
  [9],
  [3,15],
  [20],
  [7]
]
```

**Examples 2:**

```text
Input: [3,9,8,4,0,1,7]

     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7 

Output:

[
  [4],
  [9],
  [3,0,1],
  [8],
  [7]
]
```

**Examples 3:**

```text
Input: [3,9,8,4,0,1,7,null,null,null,2,5] (0's right child is 2 and 1's left child is 5)

     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7
    /\
   /  \
   5   2

Output:

[
  [4],
  [9,5],
  [3,0,1],
  [8,2],
  [7]
]
```

## Solution

唉，又是一条做过的题，问题的关键是如何保证从上到下，从左到右的顺序。

我想一想啊，感觉可以用BFS，首先保证了从上到下的顺序，同时也保证了相同位置的点的从左到右的顺序。当然，为了确定点离中心线的位移，我们可以选择封装一个额外的位移变量。

高票答案和我想的一样，不过，他将额外的column位移变量单独存在一一对应的Queue当中。

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
    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        Queue<TreeNode> nodes = new LinkedList<>();
        Queue<Integer> cols = new LinkedList<>();
        nodes.offer(root);
        cols.offer(0);
        
        Map<Integer, List<Integer>> map = new HashMap<>();
        int mincol = 0;
        int maxcol = 0;
        while (!nodes.isEmpty()) {
            TreeNode node = nodes.poll();
            int col = cols.poll();
            mincol = Math.min(mincol, col);
            maxcol = Math.max(maxcol, col);
            map.computeIfAbsent(col, x -> new ArrayList<>()).add(node.val);
            if (node.left != null) {
                nodes.offer(node.left);
                cols.offer(col - 1);
            }
            if (node.right != null) {
                nodes.offer(node.right);
                cols.offer(col + 1);
            }
        }
        while (mincol <= maxcol) {
            res.add(map.get(mincol++));
        }
        return res;
    }
}
```


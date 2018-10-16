# All Nodes Distance K in Binary Tree

## Description

We are given a binary tree \(with root node `root`\), a `target` node, and an integer value `K`.

Return a list of the values of all nodes that have a distance `K` from the `target` node.  The answer can be returned in any order.



**Example 1:**

```text
Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2

Output: [7,4,1]

Explanation: 
The nodes that are a distance 2 from the target node (with value 5)
have values 7, 4, and 1.

```

![](../.gitbook/assets/image%20%281%29.png)

\*\*\*\*

```text
Note that the inputs "root" and "target" are actually TreeNodes.
The descriptions of the inputs above are just serializations of these objects.
```

**Note:**

1. The given tree is non-empty.
2. Each node in the tree has unique values `0 <= node.val <= 500`.
3. The `target` node is a node in the tree.
4. `0 <= K <= 1000`.

## **Solution**

这道题的精妙之处就在于如何巧妙地将树转化成图做BFS

用map存子节点到父节点的映射，然后我们就可以做BFS了

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
    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        Map<TreeNode, TreeNode> parent = new HashMap<>();   
        dfs(root, null, parent);
        
        Queue<TreeNode> queue = new LinkedList<>();
        Set<TreeNode> visited = new HashSet<>();
        visited.add(target);
        queue.offer(target);
        
        int level = 0;
        List<Integer> result = new LinkedList<>();
    
        while (!queue.isEmpty() && level <= K) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                if (level == K) result.add(cur.val);
                
                TreeNode pa = parent.get(cur);
                if (pa != null && !visited.contains(pa)) {
                    queue.offer(pa);
                    visited.add(pa);
                }
                if (cur.left != null && !visited.contains(cur.left)) {
                    queue.offer(cur.left);
                    visited.add(cur.left);
                }
                if (cur.right != null && !visited.contains(cur.right)) {
                    queue.offer(cur.right);
                    visited.add(cur.right);
                }
            }
            level++;
        }
        return result;
    }
    
    private void dfs(TreeNode root, TreeNode pa, Map<TreeNode, TreeNode> parent) {
        if (root == null) return;
        parent.put(root, pa);
        dfs(root.left, root, parent);
        dfs(root.right, root, parent);
    }
}
```


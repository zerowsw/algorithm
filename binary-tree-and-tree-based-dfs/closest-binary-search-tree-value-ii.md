---
description: 搞了我大半天，有时间多写几次，体会一下其中的思维过程
---

# Closest Binary Search Tree Value II

## Description

Given a non-empty binary search tree and a target value, find `k` values in the BST that are closest to the target.

## Example

Given root = `{1}`, target = `0.000000`, k = `1`, return `[1]`.

## Challenge

Assume that the BST is balanced, could you solve it in less than O\(n\) runtime \(where n = total nodes\)?

## Solution

相对而言比较简单的做法，就是先inorder遍历，然后找最近的K个元素，时间复杂度为O\(n\)

好吧，这道题我不会做，需要参考一下：[https://www.jiuzhang.com/solution/closest-binary-search-tree-value-ii/](https://www.jiuzhang.com/solution/closest-binary-search-tree-value-ii/)

嗯，亮瞎了，从代码行数来看，这是一道真正的hard。。学学吧

所以基本思路如下：

1. 首先getStack\(\), 假装在BST中查找Target的位置，将一路经过的点存入Stack用于后续iterate
2. moveUpper\(\), 将其当作数组处理之后，需要从target位置向两边扩展，这就相当于i++
3. moveLower\(\), 同样的道理，这就相当于i--

思路充分体现了处理很多hard问题的方法，也就是先列解决问题的总框架，然后再想办法解决其中涉及的小问题。

其实这里的moveUpper和moveLower就是两道我之前做过的easy题。

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */

public class Solution {
    /**
     * @param root: the given BST
     * @param target: the given target
     * @param k: the given k
     * @return: k values in the BST that are closest to the target
     */
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        // write your code here
        Stack<TreeNode> lower = getStack(root, target);
        Stack<TreeNode> upper = new Stack<>();
        upper.addAll(lower);
        
        List<Integer> list = new ArrayList<>();
        //move element of lower or upper to suitable position
        if (lower.peek().val < target) {
            moveUpper(upper);
        } else {
            moveLower(lower);
        }
        
        for (int i = 0; i < k; i++) {
            //这个逻辑的处理也很漂亮，把需要上移的逻辑单独列出来
            if (lower.isEmpty() || !upper.isEmpty() && target - lower.peek().val > upper.peek().val - target ) {
                list.add(upper.peek().val);
                moveUpper(upper);
            } else {
                list.add(lower.peek().val);
                moveLower(lower);
            }
        }
        return list;
    }
    
    private Stack<TreeNode> getStack(TreeNode root, double target) {
        Stack<TreeNode> stack = new Stack<>();
        while (root != null) {
            stack.push(root);
            if (root.val < target) {
                root = root.right;
            } else {
                root = root.left;
            }
        }
        return stack;
    }
    
    //关键的两个function，moveUpper & moveLower, 其余部分倒是很好处理
    private void moveUpper(Stack<TreeNode> upper) {
        TreeNode peek = upper.peek();
        if (peek.right == null) {
            upper.pop();
            while (!upper.isEmpty() && upper.peek().right == peek) {
                peek = upper.pop();
            }
            return;
        }
        TreeNode right = upper.peek().right;
        while (right != null) {
            upper.push(right);
            right = right.left;
        }
    }
    
    private void moveLower(Stack<TreeNode> lower) {
        TreeNode peek = lower.peek();
        if (peek.left == null) {
            lower.pop();
            while (!lower.isEmpty() && lower.peek().left == peek) {
                peek = lower.pop();
            }
            return;
        }
        TreeNode left = lower.peek().left;
        while (left != null) {
            lower.push(left);
            left = left.right;
        }
    }
}
```




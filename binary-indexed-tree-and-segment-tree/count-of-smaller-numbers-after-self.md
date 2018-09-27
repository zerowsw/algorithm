---
description: FB全职店面就跪在这道题上了，唉，心中的痛
---

# Count of Smaller Numbers After Self

## Description

You are given an integer array nums and you have to return a new counts array. The counts array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

**Example:**

```text
Input: [5,2,6,1]
Output: [2,1,1,0] 
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```

## Solution

唉，FB就跪在这道题上面了，先写个，binary search tree的版本吧：

```java
class Solution {
    
    private class BSTNode{
        int val;
        int count;
        BSTNode right;
        BSTNode left;
        public BSTNode(int val, int count) {
            this.val = val;
            this.count = count;
        }
    } 
    
    private BSTNode insert(BSTNode root, int val) {
        if (root == null) return new BSTNode(val, 1);
        if (root.val == val) {
            root.count++;
            return root;
        } else if (root.val < val) {
            root.right = insert(root.right, val);
            return root;
        } else {
            root.count++;
            root.left = insert(root.left, val);
            return root;
        }
    }
    
    private int search(BSTNode root, int val) {
        if (root == null) return 0;
        if (root.val == val) {
            return root.count;
        } else if (root.val > val) {
            return search(root.left, val);
        } else {
            return root.count + search(root.right, val);
        }
    }
    
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> result = new ArrayList<>();
        if (nums == null || nums.length == 0) return result;
        BSTNode root = new BSTNode(nums[nums.length - 1], 1);
        result.add(0);
        for (int i = nums.length - 2; i >= 0; i--) {
            result.add(search(root, nums[i] - 1));
            root = insert(root, nums[i]);
        }
        Collections.reverse(result);
        return result;
    }
}
```

然后就是使用BIT来做，你要明白本质上BIT是为了解决prefix sum的问题的，所有你要在原问题的基础上进行一定的转化。

不能说找后面比它小的元素个数，而应该找前面比它小的，这样才能用prefix sum。

同时你要使用prefix sum,这个sum应该是当前位置之前的，比他小的个数之和。

总之，用0 ~ K 表示数组里面unique number的大小关系，然后从前到后构建BIT即可。

```java
class Solution {
    private class FenwickTree{
        int[] tree;
        
        public FenwickTree(int size) {
            tree = new int[size];
        }
        
        public void update(int i, int delta) {
            while (i < tree.length) {
                tree[i] += delta;
                i += lowBit(i);
            }
        }
        
        public int sum(int i) {
            int sum = 0;
            while (i > 0) {
                sum += tree[i];
                i -= lowBit(i);
            }
            return sum;
        }
        
        private int lowBit(int x) {
            return x & (-x);
        }
    }
    
    
    public List<Integer> countSmaller(int[] nums) {
        
        TreeSet<Integer> set = new TreeSet<>();
        for (Integer num : nums) {
            set.add(num);
        }
        Map<Integer, Integer> map = new HashMap<>();
        int index = 1;
        for (Integer num : set) {
            map.put(num, index++);
        }
    
        reverse(nums);
        
        List<Integer> result = new ArrayList<>();
        FenwickTree tree = new FenwickTree(set.size() + 1);
        for (Integer num : nums) {
            int order = map.get(num);
            int smaller = tree.sum(order - 1);
            result.add(smaller);
            tree.update(order, 1);
        }
        Collections.reverse(result);
        return result;
    }
    
    private void reverse(int[] array) {
        int le = 0;
        int ri = array.length - 1;
        while (le < ri) {
            int temp = array[le];
            array[le] = array[ri];
            array[ri] = temp;
            le++;
            ri--;
        }
    }
}
```

这道题的merge sort方法先放一放。。




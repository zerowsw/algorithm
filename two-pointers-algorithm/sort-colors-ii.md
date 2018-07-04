# Sort Colors II

## Description

Given an array of _n_ objects with _k_ different colors \(numbered from 1 to k\), sort them so that objects of the same color are adjacent, with the colors in the order 1, 2, ... k.

## Example

Given colors=`[3, 2, 2, 1, 4]`, `k=4`, your code should sort colors in-place to `[1, 2, 2, 3, 4]`.

## Challenge

A rather straight forward solution is a two-pass algorithm using counting sort. That will cost O\(k\) extra memory. Can you do it without using extra memory?

## Solution

首先就像他说的那样，用counting sort可以很容易地解决这个问题。关键是想一想如何不用O\(k\)的extra memory。 

其实把，简单想一想，他没有说明K的大小，如果K很大的话， 明显只能用一般的排序算法来处理。只有当K比较小的时候，比如说K&lt;=3的情况下，我们可以使用Two pointers， 原地处理。

卧槽，Rainbow Sort是什么鬼？？

绝妙的想法，首先要确认, 颜色的表示，这里明确表示是用1到K的值来表示颜色。然后就是利用类似快速排序的思路，不过分界值不太是随机选择的viot，而是颜色值的中值点。 快速排序的时间复杂度是nlogn, 而这里的排序速度可以想象是nlogk, 因为只有k种不同的颜色，所以往下递归处理的深度。。。。

这种题目一般都是非常适合用来思考边界条件的

```java
public class Solution {
    /**
     * @param colors: A list of integer
     * @param k: An integer
     * @return: nothing
     */
    public void sortColors2(int[] colors, int k) {
        // write your code here
        if (colors == null || colors.length == 0) return;
        helper(colors, 0, colors.length - 1, 1, k);
    }
    
    public void helper(int[] colors, int left, int right, int colorf, int colort) {
        
        if (left >= right) return;
        //这里的终止条件需要再想一想，如果用>的话，会无法终止
        if (colorf == colort) return;
        int le = left;
        int ri = right;
        int colorm = (colorf + colort) / 2;
        while (le <= ri) {
            while (le <= ri && colors[le] <= colorm) {
                le++;
            }
            while (le <= ri && colors[ri] > colorm) {
                ri--;
            }
            if (le <= ri) {
                int temp = colors[ri];
                colors[ri] = colors[le];
                colors[le] = temp;
                le++;
                ri--;
            }
        }
        helper(colors, left, ri, colorf, colorm);
        //这里，需要加1，比如colorf和colort已经相邻的情况，不加1的话是永远也没法用到colort的
        helper(colors, le, right, colorm + 1, colort);
    }
}
```

解法1无疑是非常精妙的，另外的话，还有一个two pointers的解法，虽然时间复杂度是nk，会超时，但是思路比较直接，算是除了整体排序之外，最为直接的算法的了。

```java
public class Solution {
    /**
     * @param colors: A list of integer
     * @param k: An integer
     * @return: nothing
     */
    public void sortColors2(int[] colors, int k) {
        // write your code here
        if (colors == null || colors.length == 0) return;
        int le = 0;
        int ri = colors.length - 1;
        while (le <= ri) {
            int min = Integer.MAX_VALUE;
            int max = Integer.MIN_VALUE;
            for (int i = le; i <= ri; i++) {
                min = Math.min(min, colors[i]);
                max = Math.max(max, colors[i]);
            }
            
            int cur = le;
            while (cur <= ri) {
                if (colors[cur] == min) {
                    swap(colors, cur, le++);
                } else if (colors[cur] == max) {
                    swap(colors, cur, ri--);
                }
                cur++;
            }
        }
    }
    
    public void swap(int[] colors, int i, int j) {
        int temp = colors[j];
        colors[j] = colors[i];
        colors[i] = temp;
    }
}
```




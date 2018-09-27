# Set Matrix Zeroes

## Description

Given a _m_ x _n_ matrix, if an element is 0, set its entire row and column to 0. Do it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

```text
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

**Example 2:**

```text
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

**Follow up:**

* A straight forward solution using O\(_mn_\) space is probably a bad idea.
* A simple improvement uses O\(_m_ + _n_\) space, but still not the best solution.
* Could you devise a constant space solution?

## Solution

比较简单直接的方法，就是另外建一个矩阵，空间O\(mn\),  或者用两个额外的数组啥的，记录哪些行哪些列应该置0。

想要进一步优化空间的话，我比较naive的想法，是先用标签把那些位置标记一下，然而数值范围是整个Integer的话，这也不行。

那么，参考discussion之后得到的比较tricky的方法，就是用第一行和第一列来标记0。。




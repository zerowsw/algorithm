---
description: 打个卡而已
---

# Combinations

## Description

Given two integers _n_ and _k_, return all possible combinations of _k_ numbers out of 1 ... _n_.

## Example

Given `n = 4` and `k = 2`, a solution is:

```text
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4]
]
```

## Solution

没啥好说的

```java
public class Solution {
    /**
     * @param n: Given the range of numbers
     * @param k: Given the numbers of combinations
     * @return: All the combinations of k numbers out of 1..n
     */
    public List<List<Integer>> combine(int n, int k) {
        // write your code here
        List<List<Integer>> res = new ArrayList<>();
        helper(res, new ArrayList<>(), n, k , 1, 0);
        return res;
    }
    
    public void helper(List<List<Integer>> res, List<Integer> cur, int n, int k, int index, int count) {
        if (count == k) {
            res.add(new ArrayList<>(cur));
            return;
        }
        if (index > n) {
            return;
        }
        
        for (int i = index; i <= n; i++) {
            cur.add(i);
            helper(res, cur, n, k, i + 1, count + 1);
            cur.remove(cur.size() - 1);
        }
    } 
}
```


# Combination Sum II

## Description

Given a collection of candidate numbers \(_C_\) and a target number \(_T_\), find all unique combinations in _C_ where the candidate numbers sums to _T_.

Each number in _C_ may only be used once in the combination.

## Example

Given candidate set `[10,1,6,7,2,1,5]` and target `8`,

A solution set is:

```text
[
  [1,7],
  [1,2,5],
  [2,6],
  [1,1,6]
]
```

## Solution

find all combination, 这种一看到，基本想都不用想的DFS了。

然后，不要被吓住，稍微一想，其实就会发现，很简单，没啥好说的。

```java
public class Solution {
    /**
     * @param num: Given the candidate numbers
     * @param target: Given the target number
     * @return: All the combinations that sum to target
     */
    public List<List<Integer>> combinationSum2(int[] num, int target) {
        // write your code here
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(num);
        helper(res, new ArrayList<>(), num, 0, target);
        return res;
    }
    
    public void helper(List<List<Integer>> res, List<Integer> cur, int[] num, int index, int remain) {
        if (remain == 0) {
            res.add(new ArrayList<>(cur));
            return;
        }
        if (index >= num.length || remain < 0) {
            return;
        }

        
        for (int i = index; i < num.length; i++) {
            if (i > index && num[i] == num[i - 1]) continue;
            cur.add(num[i]);
            helper(res, cur, num, i + 1, remain - num[i]);
            cur.remove(cur.size() - 1);
        }
    }
}
```




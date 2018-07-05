---
description: 基础题，练练手
---

# 3 Sum

## Description

Given an array _S_ of n integers, are there elements _a_, _b_, _c_ in _S_ such that `a + b + c = 0`? Find all unique triplets in the array which gives the sum of zero.

## Example



For example, given array S = `{-1 0 1 2 -1 -4}`, A solution set is:

```text
(-1, 0, 1)
(-1, -1, 2)
```

## Solution

没啥好说的，要想找到所有Unique的tuple, 关键是去除。 显然我已经深谙其道了。（最好是把two sum的部分提取出来单独写成一个函数，我这里为了图方便，直接放在了一起）。

```java
public class Solution {
    /**
     * @param numbers: Give an array numbers of n integer
     * @return: Find all unique triplets in the array which gives the sum of zero.
     */
    public List<List<Integer>> threeSum(int[] numbers) {
        // write your code here
        List<List<Integer>> res = new ArrayList<>();
        if (numbers == null || numbers.length < 3) return res;
        
        Arrays.sort(numbers);
        
        for (int i = 0; i < numbers.length - 2; i++) {
            if (i != 0 && numbers[i] == numbers[i - 1]) continue;
            int le = i + 1;
            int ri = numbers.length - 1;
            int target = -numbers[i];
         
            while (le < ri) {
                if (le != i + 1 && numbers[le] == numbers[le - 1]) {
                    le++;
                    continue;
                }
                if (ri != numbers.length - 1 && numbers[ri] == numbers[ri + 1]) {
                    ri--;
                    continue;
                }
                if (numbers[le] + numbers[ri] == target) {
                    List<Integer> tuple = new ArrayList<>();
                    tuple.add(numbers[i]);
                    tuple.add(numbers[le]);
                    tuple.add(numbers[ri]);
                    res.add(tuple);
                    le++;
                    ri--;
                } else if (numbers[le] + numbers[ri] < target) {
                    le++;
                } else {
                    ri--;
                }
            }
        }
        return res;
    }
}
```


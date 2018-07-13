---
description: 打卡题，很基础。。
---

# N-Queens II

## Description

Follow up for N-Queens problem.

Now, instead outputting board configurations, return the total number of distinct solutions.

## Example

For n=4, there are 2 distinct solutions.

## Solution

nothing special

```java
public class Solution {
    /**
     * @param n: The number of queens.
     * @return: The total number of distinct solutions.
     */
    
    int count = 0; 
    public int totalNQueens(int n) {
        // write your code here
        if (n <= 0) return 0;
        helper(new ArrayList<>(), n);
        return count;
    }
    
    private void helper(List<Integer> cur, int n) {
        if (cur.size() == n) {
            count++;
            return;
        }
        
        for (int i = 1; i <= n; i++) {
            if (!check(cur, i)) continue;
            cur.add(i);
            helper(cur, n);
            cur.remove(cur.size() - 1);
        }
    }
    
    private boolean check(List<Integer> prev, int newP) {
        int s = prev.size();
        for (int i = 1; i <= s; i++) {
            int j = prev.get(i - 1);
            if (newP == j || Math.abs(newP - j) == s + 1 - i) return false;
        }
        return true;
    }
}
```


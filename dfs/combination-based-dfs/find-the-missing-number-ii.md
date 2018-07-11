# Find the Missing Number II

## Description

Giving a string with number from 1-`n` in random order, but miss `1` number.Find that number.

## Example

Given n = `20`, str = `19201234567891011121314151618`

return `17`

## Solution

思路基本上还是比较直接的。不过，还是有corner case没有考虑到, 在即便限制了数量为N - 1， 每个数都不重复，并且都小于等于N，还是有可能出现不同的组合。比如109， 可以当成10和9， 或 1和09。 09的情况需要额外判断。

```java
public class Solution {
    /**
     * @param n: An integer
     * @param str: a string with number from 1-n in random order and miss one number
     * @return: An integer
     */
    public int findMissing2(int n, String str) {
        // write your code here
        Set<Integer> set = new HashSet<>();
        int digi = 0;
        int temp = n;
        while (temp > 0) {
            digi++;
            temp /= 10;
        }
        helper(set, new HashSet<>(), str, digi, n, 0);
        for (int i = 1; i <= n; i++) {
            if (!set.contains(i)) return i;
        }
        return -1;
    }
    
    public void helper(Set<Integer> set, Set<Integer> cur, String str, int digi, int n, int index) {
        if (index >= str.length()) {
            if (cur.size() == n - 1) {
                set.addAll(cur);
            }
            return;
        }
        
        for (int i = 1; i <= digi; i++) {
            if (index + i > str.length()) return;
            int next = Integer.parseInt(str.substring(index, index + i));
            if (next > n) continue;
            if (cur.contains(next)) continue;
            if (next < Math.pow(10, i - 1)) continue;
            Set<Integer> nes = new HashSet<>();
            nes.addAll(cur);
            nes.add(next);
            helper(set, nes, str, digi, n, index + i);
        }
    }
}
```




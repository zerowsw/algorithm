# Expression Add Operators

## Description

Given a string that contains only digits `0-9` and a target value, return all possibilities to add **binary** operators \(not unary\) `+`, `-`, or `*`between the digits so they evaluate to the target value.

## Example

**Example 1:**

```text
Input: num = "123", target = 6
Output: ["1+2+3", "1*2*3"] 
```

**Example 2:**

```text
Input: num = "232", target = 8
Output: ["2*3+2", "2+3*2"]
```

**Example 3:**

```text
Input: num = "105", target = 5
Output: ["1*0+5","10-5"]
```

**Example 4:**

```text
Input: num = "00", target = 0
Output: ["0+0", "0-0", "0*0"]
```

**Example 5:**

```text
Input: num = "3456237490", target = 9191
Output: []
```

## Solution

妈蛋，脑子一团浆糊了，算了，直接看解答吧。。

咦，怎么感觉是相当brute force的方法，复杂度爆表，可以看一下分析（我觉得他的分析有问题啊）

这道题跳过（方法上我觉得没有太大参考价值）

[https://leetcode.com/problems/expression-add-operators/solution/](https://leetcode.com/problems/expression-add-operators/solution/)

参考了递归的解法之后，得到相当简洁的解答方法：

```java
class Solution {
    public List<String> addOperators(String num, int target) {
        List<String> result = new ArrayList<>();
        helper(result, "", num, 0, 0, 0, target);
        return result;
    }
    
    public void helper(List<String> res, String formula, String num, int start, long sum, long LastFactor, int target) {
        if (start == num.length()) {
            if (sum == target) {
                res.add(formula);
            }
            return;
        }
        for (int i = start; i < num.length(); i++) {
            long newnum = Long.parseLong(num.substring(start, i + 1));
            if (start == 0) {
                helper(res, "" + newnum, num, i + 1, newnum, newnum, target);
            } else {
                helper(res, formula + "+" + newnum, num, i + 1, sum + newnum, newnum, target);
                helper(res, formula + "*" + newnum, num, i + 1, sum - LastFactor + LastFactor * newnum, LastFactor * newnum, target);
                helper(res, formula + "-" + newnum, num, i + 1, sum - newnum, -newnum, target); 
            }
            if (newnum == 0) break;
        }
    }
}
```

时间复杂度分析：

The process moving to N length of string gives us from 3_T\(n-1\) to 3_T\(1\) :  
T\(n\) = 3 \* T\(n-1\) + 3 \* T\(n-2\) + 3 \* T\(n-3\) + ... + 3 \*T\(1\);  
T\(n-1\) = 3 \* T\(n-2\) + 3 \* T\(n-3\) + ... 3 \* T\(1\);  
Thus T\(n\) = 4T\(n-1\);

所以，你懂的。。


# Divide Two Integers

## Description

Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero.

## Example

**Example 1:**

```text
Input: dividend = 10, divisor = 3
Output: 3
```

**Example 2:**

```text
Input: dividend = 7, divisor = -3
Output: -2
```

## Note

* Both dividend and divisor will be 32-bit signed integers.
* The divisor will never be 0.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: \[−231,  231 − 1\]. For the purpose of this problem, assume that your function returns 231 − 1 when the division result overflows.

## Solution

乍一看，感觉有点懵逼，以为是位运算，仔细一想，乘法可以用加法实现，那么除法也可以啊。同时，可以用binary search简化加法的过程。 

注意Corner Case

```java
class Solution {
    public int divide(int dividend, int divisor) {
        
        boolean negative = false;
        if (dividend < 0 && divisor > 0 || dividend > 0 && divisor < 0) {
            negative = true;
        } 
        //since I decide to use binary search, to avoid overflow, transfer them into long
        long divd = Math.abs((long)dividend);
        long divs = Math.abs((long)divisor);
        int res = helper(divd, divs);
        
        //有可能overflow, 这时res == Integer.MIN_VALUE;
        if (res < 0 && negative) return res;
        if (res < 0) return Integer.MAX_VALUE;
        
        if (negative) return 0 - res;
        return res;
    }
    
    private int helper(long divd, long divs) {
        if (divd < divs) return 0;
        long sum = divs;
        int count = 1;
        while (sum + sum < divd) {
            sum += sum;
            count += count;
        }
        return count + helper(divd - sum, divs);
    }
}
```


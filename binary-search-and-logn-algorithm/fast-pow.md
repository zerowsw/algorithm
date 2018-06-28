---
description: 幂取模算法，即幂模算法 （我觉得不是那么重要，说实话，碰到的概率很低）
---

# Fast Pow

## Description

Calculate the **an % b** where a, b and n are all 32bit integers.

## Example

For 231 % 3 = 2

For 1001000 % 1000 = 0

## Challenge

O\(logn\)

## Solution

a 与 c 同余，则ab 与 bc 同余， 则 ab % m = b \* \(a % m \) % m

所以 a ^ n % m  =  a  \*  \(a \* \(........\) % m\) % m, 这种形式就很容易写成代码解决了，时间复杂度为O\(n\), 当然，这里有个非常重要的问题要解决，就是当指数为0的时候，并且是模1的时候（太特殊了吧。。），结果为0

然而，我们追求的是时间复杂度为O\(logn\)

给个链接自己看吧：

> [https://blog.csdn.net/chen77716/article/details/7093600](https://blog.csdn.net/chen77716/article/details/7093600)

还有个问题，这里面的计算是有越界的可能的，所以我们需要用long类型的变量来协助运算

```java
public class Solution {
    /**
     * @param a: A 32bit integer
     * @param b: A 32bit integer
     * @param n: A 32bit integer
     * @return: An integer
     */
    public int fastPower(int a, int b, int n) {
        // write your code here
        long ans = 1, tmp = a;
        
        while (n != 0) {
            if (n % 2 == 1) {
                ans = (ans * tmp) % b;
            }
            tmp = (tmp * tmp) % b;
            n = n / 2;
        }  
        return (int) ans % b;
    }
}
```


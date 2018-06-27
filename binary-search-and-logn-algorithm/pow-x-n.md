---
description: 高频题，纯属练练手
---

# Pow\(x, n\)

## Description

Implement pow\(x, n\).

## Example



```text
Pow(2.1, 3) = 9.261
Pow(0, 1) = 0
Pow(1, 0) = 1
```

## Challenge

O\(logn\) time

## Solution

想当然地考虑不周啊，指数难道不能是负数？

并且我最初想到的解法居然超时了了。。。。。如下：

而且代码是有问题的，你做n= -n 时居然忘了java int 上下边界不对称。。是不是傻

```java
public class Solution {
    /*
     * @param x: the base number
     * @param n: the power number
     * @return: the result
     */
    public double myPow(double x, int n) {
        // write your code here
        if (n == 0) return 1.0;
        boolean flag = false;
        if (n < 0) {
            flag = true;
            n = -n;
        }
        
        double temp = x;
        int c = 1;
        while (c * 2 < n) {
            temp = temp * temp;
            c *= 2;
        }
        
        if (flag) {
            return 1 / (temp * myPow(x, n - c));
        } else {
            return temp * myPow(x, n - c);
        }
    }
}
```

于是乎，思考怎么样进一步的进行优化。当前的时间复杂度并不是真正意义上的O\(logn\)，可以推算一下：

T\(n\) = T\(n/2\) + logn （大概），所以最后的时间复杂度大概是O\(logn^2\) 

另外怎么处理n = Integer.MIN\_VALUE 的情况也是一个值得思考的问题。

答案的解题思路正是从如何保存并利用之前的计算结果的角度入手的。其实就是模拟将n转换成二进制，同时记录在对应1的位置的值，相乘即可。 比如Pow\(x, 17\),  n = 17, 即10001， 所以对应的就是\(x ^ 16\) \* x

这个算法的核心还是在于，怎么样利用十进制二进制转化算法。至于我之前的算法，每次递归的时候，都要重新计算一次，因此时间复杂度就高了一些。 

```java
public class Solution {
    /*
     * @param x: the base number
     * @param n: the power number
     * @return: the result
     */
    public double myPow(double x, int n) {
        // write your code here
        boolean isNegative = false;
        if (n < 0) {
            isNegative = true;
            n = -(n + 1);
            x = 1 / x;
        }
        
        double temp = x;
        double res = 1.0;
        while (n != 0) {
            if (n % 2 == 1) {
                res *= temp;
            }
            n /= 2;
            temp *= temp;
        }
        
        if (isNegative) {
            return res * x;
        }
        return res;
    }
}
```




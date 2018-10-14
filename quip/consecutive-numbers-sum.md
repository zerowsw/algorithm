---
description: 感觉sliding window这类题我一直在采用一个非常不好的模版在解题。。
---

# Consecutive Numbers Sum

## Description

Given a positive integer `N`, how many ways can we write it as a sum of consecutive positive integers?

## Example

**Example 1:**

```text
Input: 5
Output: 2
Explanation: 5 = 5 = 2 + 3
```

**Example 2:**

```text
Input: 9
Output: 3
Explanation: 9 = 9 = 4 + 5 = 2 + 3 + 4
```

**Example 3:**

```text
Input: 15
Output: 4
Explanation: 15 = 15 = 8 + 7 = 4 + 5 + 6 = 1 + 2 + 3 + 4 + 5
```

**Note:** `1 <= N <= 10 ^ 9`.

## Solution

首先，可以确定，这个数字序列的范围应该是在1 - N／2。

然后，就可以用sliding window来做了。

版本迭代：

错误，没有处理1的情况

```text
class Solution {
    public int consecutiveNumbersSum(int N) {
        int count = 1;
        int left = 1;
        int right = 1;
        int sum = 0;
        while (right <= N / 2 + 1) {
            if (sum < N) {
                sum += right;
                right++;
            } 
            if (sum > N) {
                sum -= left++;
            }
             if (sum == N) {
                count++;
                sum -= left++;
            }
        }
        return count;
    }
}
```

下一个版本，超时： （猜测一下，我估计是中间结果溢出。。）

```java
class Solution {
    public int consecutiveNumbersSum(int N) {
        int count = 1;
        int left = 1;
        int right = 1;
        int sum = 0;
        while (left <= N / 2) {
            if (sum < N) {
                sum += right++;
            } else if (sum > N) {
                sum -= left++;
            } else if (sum == N) {
                count++;
                sum -= left++;
            }
        }
        return count;
    }
}
```

卧槽，改成long之后还是超时。。难道还有更优的解法？？？

OK, 还有根号N复杂度的方法，跟我一开始设想的用公式推导的限制相关。

K + 1, K + 2,  ....,  K + i    和为 \(2 \* K + 1 + i\) \* i  / 2,   然后我们可以限制i &lt; Math.pow\(2 \* N, 0.5\)

```java
class Solution {
    public int consecutiveNumbersSum(int N) {
        double upbound = Math.pow(2 *N, 0.5);
        int count = 0;
        for (int i = 1; i < upbound; i++) {
            if (2 * N % i == 0) {
                int k = (2 * N) / i - (i + 1);
                if (k >= 0 && k % 2 == 0) {
                    count++;
                }
            }
        }
        return count;
    }
}
```


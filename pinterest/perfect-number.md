# Perfect Number

## Description

We define the Perfect Number is a **positive** integer that is equal to the sum of all its **positive** divisors except itself.Now, given an **integer** n, write a function that returns true when it is a perfect number and false when it is not.

**Example:**

```text
Input: 28
Output: True
Explanation: 28 = 1 + 2 + 4 + 7 + 14
```

**Note:** The input number **n** will not exceed 100,000,000. \(1e8\)

## Solution

我居然脑残般地从1算到N，，你是傻的嘛。。

```java
class Solution {
    public boolean checkPerfectNumber(int num) {
        if (num <= 0) return false;
        int sum = 1;
        if (num == 1) return false;
        int sqr = (int)Math.sqrt(num);
        for (int i = 2; i <= sqr; i++) {
            if (num % i == 0) {
                sum += i;
                sum += (num / i);
            }
        }
        return sum == num;
    }
}
```


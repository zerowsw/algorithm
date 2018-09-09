# Multiply Strings

## Description

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

## Example

**Example 1:**

```text
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```text
Input: num1 = "123", num2 = "456"
Output: "56088"
```

## Solution

思路较为直白，转化成char array, 一个个的来处理就可以。

搞错了，当成加法来做了。

每次两个数相乘，乘积都可以加到结果对应位置上。

m位数乘以n位数，结果最多是（m + n）位数。

我的没有优化的废代码：

```java
class Solution {
    public String multiply(String num1, String num2) {
        char[] array1 = num1.toCharArray();
        char[] array2 = num2.toCharArray();
        int m = array1.length;
        int n = array2.length;
        int[] res = new int[m + n];
        StringBuilder result = new StringBuilder();
        for(int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                int a = array1[i] - '0';
                int b = array2[j] - '0';
                int product = a * b;
                int c = res[i + j + 1];
                res[i + j + 1] = (product % 10 + c) % 10;
                int d = (product % 10 + c) / 10;
                int two = product / 10 + d + res[i + j];
                res[i + j] = two % 10;
                if (two / 10 > 0) res[i + j - 1] += two / 10;
            }
        }
        
        if (res[0] == 0 && res[1] == 0) return "0";
        for (int i = 0; i < m + n; i++) {
            if (i == 0 && res[i] == 0) continue;
            result.append(res[i] + "");
        }
        return result.toString();
    }
}
```

实际上不用考虑加法的进位，简化的解法：

```java
public String multiply(String num1, String num2) {
    int m = num1.length(), n = num2.length();
    int[] pos = new int[m + n];
   
    for(int i = m - 1; i >= 0; i--) {
        for(int j = n - 1; j >= 0; j--) {
            int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0'); 
            int p1 = i + j, p2 = i + j + 1;
            int sum = mul + pos[p2];

            pos[p1] += sum / 10;
            pos[p2] = (sum) % 10;
        }
    }  
    
    StringBuilder sb = new StringBuilder();
    for(int p : pos) if(!(sb.length() == 0 && p == 0)) sb.append(p);
    return sb.length() == 0 ? "0" : sb.toString();
}
```


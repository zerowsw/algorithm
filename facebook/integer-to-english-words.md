# Integer to English Words

## Description

Convert a non-negative integer to its english words representation. Given input is guaranteed to be less than 231 - 1.

## Example

**Example 1:**

```text
Input: 123
Output: "One Hundred Twenty Three"
```

**Example 2:**

```text
Input: 12345
Output: "Twelve Thousand Three Hundred Forty Five"
```

**Example 3:**

```text
Input: 1234567
Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
```

**Example 4:**

```text
Input: 1234567891
Output: "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety 
```

## Solution

没啥好说的，写helper函数很好地处理了0的情况。String.trim\(\) 能删除首尾出现的空格字符。

```java
class Solution {
    
    public final String[] digits = {"Zero", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten"};
    public final String[] tens = {"Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
    public final String[] hehe = {"Eleven","Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    public final int Billion = 1000000000;
    public final int Million = 1000000;
    public final int Thousand = 1000;
    public final int Hundred = 100;
    
    public String numberToWords(int num) {
        if (num == 0) return "Zero";
        String res = helper(num);
        return res.trim();
    }
    
    public String helper(int num) {
        StringBuilder builder = new StringBuilder();
        if (num >= 1000000000) {
            String left = helper(num / Billion);
            builder.append(left).append("Billion ");
            builder.append(helper(num % Billion));
        } else if (num >= 1000000) {
            builder.append(helper(num / Million)).append("Million ").append(helper(num % Million));
        } else if (num >= 1000) {
            builder.append(helper(num / Thousand)).append("Thousand ").append(helper(num % Thousand));
        } else if (num >= 100) {
            builder.append(helper(num / Hundred)).append("Hundred ").append(helper(num % Hundred));
        } else if (num >= 20) {
            builder.append(tens[num / 10 - 2] + " ").append(helper(num % 10));
        } else if (num > 10) {
            builder.append(hehe[num - 11] + " ");
        } else if (num == 10) {
            builder.append("Ten ");
        } else if (num > 0){
            builder.append(digits[num] + " ");
        } 
        return builder.toString();
    }
}
```


---
description: 经典的Easy题，有相应的follow up
---

# Best Time to Buy and Sell Stock

## Description

Say you have an array for which the _i_th element is the price of a given stock on day _i_.

If you were only permitted to complete at most one transaction \(i.e., buy one and sell one share of the stock\), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

## Solution

太简单，但是要注意corner case。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) return 0;
        int maxP = Integer.MIN_VALUE;
        int prelow = Integer.MAX_VALUE;
        for (int i = 0; i < prices.length; i++) {
            prelow = Math.min(prelow, prices[i]);
            maxP = Math.max(maxP, prices[i] - prelow);
        }
        return maxP;
    }
}
```


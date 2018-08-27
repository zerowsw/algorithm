# Candy

## Description

There are _N_ children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

* Each child must have at least one candy.
* Children with a higher rating get more candies than their neighbors.

What is the minimum candies you must give?

## Example

**Example 1:**

```text
Input: [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```

**Example 2:**

```text
Input: [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
             The third child gets 1 candy because it satisfies the above two conditions.
```

## Solution

这道题直观地用折线图来思考，只需要确保相邻节点的大小关系以及分发糖果的大小关系。

错误思维（来自赵文瀚）： 从左到右遍历，根据大小关系确定分发糖果数量的差值是1或-1， 最后的结果，整体平移，确保最少的糖果数量为1。

错误原因举例： \[1， 2， 3， 4， 5， 4\] 分发糖果数量其实可以是\[1, 2, 3, 4, 5, 1\]

所以，解决这道题一种可行的思路就是从所有的极值点出发（局部最高点和局部最低点）

比较直观的做法，同时也可以借鉴到Trapping Rain Water中的解法就是左扫一遍，加上右扫一遍，第一遍确保递增子序列当中的，每一个rating比前一个rating高的孩子，能得到多一个的糖果。 第二遍扫的时候，同样的操作，只不过要保证在顶点处取较大值。

用一个Array即可存储所有的中间结果（不需要两个Array）

```java
class Solution {
    public int candy(int[] ratings) {
        int[] candy = new int[ratings.length];
        Arrays.fill(candy, 1);
        for (int i = 1; i < ratings.length; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candy[i] = candy[i - 1] + 1;
            }
        }  
        for (int i = ratings.length - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candy[i] = Math.max(candy[i], candy[i + 1] + 1);
            }
        }
        int count = 0;
        for (Integer inte : candy) {
            count += inte;
        }
        return count;
    }
}
```




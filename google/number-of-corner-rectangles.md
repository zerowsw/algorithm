# Number Of Corner Rectangles

## Description

Given a grid where each entry is only 0 or 1, find the number of corner rectangles.

A _corner rectangle_ is 4 distinct 1s on the grid that form an axis-aligned rectangle. Note that only the corners need to have the value 1. Also, all four 1s used must be distinct.

**Example 1:**

```text
Input: grid = 
[[1, 0, 0, 1, 0],
 [0, 0, 1, 0, 1],
 [0, 0, 0, 1, 0],
 [1, 0, 1, 0, 1]]
Output: 1
Explanation: There is only one corner rectangle, with corners grid[1][2], grid[1][4], grid[3][2], grid[3][4].
```

**Example 2:**

```text
Input: grid = 
[[1, 1, 1],
 [1, 1, 1],
 [1, 1, 1]]
Output: 9
Explanation: There are four 2x2 rectangles, four 2x3 and 3x2 rectangles, and one 3x3 rectangle.
```

**Example 3:**

```text
Input: grid = 
[[1, 1, 1, 1]]
Output: 0
Explanation: Rectangles must have four distinct corners.
```

**Note:**

1. The number of rows and columns of `grid` will each be in the range `[1, 200]`.
2. Each `grid[i][j]` will be either `0` or `1`.
3. The number of `1`s in the grid will be at most `6000`.

## Solution

扫描每一行的pair，编码相对位置并检查之前之前在别的行，出现了多少次这样的Pair。

```java
class Solution {
    public int countCornerRectangles(int[][] grid) {
        Map<Integer, Integer> count = new HashMap<>();
        int result = 0;
        for (int[] row : grid) {
            for (int i = 0; i < row.length; i++) {
                if (row[i] == 1) {
                    for (int j = i + 1; j < row.length; j++) {
                        if (row[j] == 1) {
                            int c = count.getOrDefault(i * 200 + j,  0);
                            result += c;
                            count.put(i * 200 + j, c + 1);
                        }
                    }
                }
            }
        }
        return result;
    }
}
```




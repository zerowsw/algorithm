# Remove Invalid Parentheses

## Description

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

## Example

```text
"()())()" -> ["()()()", "(())()"]
"(a)())()" -> ["(a)()()", "(a())()"]
")(" -> [""]
```

## Solution

 糟糕， 没有办法冷静下来思考这道题。。

这道题真的是一条非常典型的DFS题了。

这类问题的核心思路还是左右括号数量相等哦。

想了想，如果用左右括号的数量来判断是否valid的话，如果当前右括号的数量多于左括号的数量的话，那么删除之前的一个右括号就可以了。如果左括号多了的话，那么先得接着往下走，到最后才能知道是不是invalid。那么问题来了，怎么删除呢？

方法一，看了九章的解答的第一条，怎么感觉。。有点brute force呢，核心仍然是用左右parentheses 数量的相等，直接上代码吧，比较好懂。（时间复杂度有点不太好分析啊）

这里我有点比较傻叉的忘了用start index来去重，同时也忘记了删除最少的invalid parentheses，

```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        List<String> result = new ArrayList<>();
        int[] count = getCount(s);
        helper(result, 0,count[0], count[1], s);
        return result;
    }
    
    private void helper(List<String> result, int start, int leftcount, int rightcount, String cur) {
        if (leftcount == 0 && rightcount == 0 && isValid(cur)) {
            result.add(cur);
            return;
        }
        for (int i = start; i < cur.length(); i++) {
            char chr = cur.charAt(i);
            if (i > start && chr == cur.charAt(i - 1)) continue;
            if (leftcount > 0 && chr == '(') {
                helper(result, i, leftcount - 1, rightcount, cur.substring(0, i) + cur.substring(i + 1, cur.length()));
            } else if (rightcount > 0 && chr == ')') {
                helper(result, i, leftcount, rightcount - 1, cur.substring(0, i) + cur.substring(i + 1, cur.length()));
            }
        }
    }
    
    private boolean isValid(String s) {
        int[] count = getCount(s);
        return count[0] == 0 && count[1] == 0;
    }
    
    private int[] getCount(String s) {
        int[] count = new int[2];
        for (Character chr : s.toCharArray()) {
            if (chr == '(') {
                count[0]++;
            } 
            if ( chr == ')'){
                if (count[0] > 0) {
                    count[0]--;
                } else {
                    count[1]++;
                }               
            }
        }
        return count;
    }
}
```

核心思想用这个就可以了，另外，还有BFS的解法。。想看的话，自己看discussion吧。（我不太清楚时间复杂度是多少）

另外，如果只要一种结果的话，可以考虑看看BFS？ 感觉DFS退不出来啊。。（直接返回结果？？好像也可以）


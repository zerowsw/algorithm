# Palindrome Partitioning

## Description

Given a string _s_, partition _s_ such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

## Example

Given s = `"aab"`, return:

```text
[
  ["aa","b"],
  ["a","a","b"]
]
```

## Solution

淡定，拿到题不要慌，其实很简单。

```java
public class Solution {
    /*
     * @param s: A string
     * @return: A list of lists of string
     */
    public List<List<String>> partition(String s) {
        // write your code here
        List<List<String>> res = new ArrayList<>();
        helper(res, new ArrayList<>(), s, 0);
        return res;
    }
    
    private void helper(List<List<String>> res, List<String> cur, String s, int index) {
        //end condition
        if (index >= s.length()) {
            res.add(new ArrayList<>(cur));
            return;
        }      
        for (int i = index; i < s.length(); i++) {
            if (checkPalindrome(s, index, i)) {
                cur.add(s.substring(index, i + 1));
                helper(res, cur, s, i + 1);
                cur.remove(cur.size() - 1);
            }
        }
    
    }
    private boolean checkPalindrome(String s, int left, int right) {
        while (left < right) {
            if (s.charAt(left++) != s.charAt(right--)) return false;
        }
        return true;
    }
}
```

还有快一些的解法2，可以抽空看一下：[https://www.jiuzhang.com/solution/palindrome-partitioning/](https://www.jiuzhang.com/solution/palindrome-partitioning/)




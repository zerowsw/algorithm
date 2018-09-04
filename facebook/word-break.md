# Word Break

## Description

Given a **non-empty** string _s_ and a dictionary _wordDict_ containing a list of **non-empty** words, determine if _s_ can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**

* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.

## Example

**Example 1:**

```text
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**

```text
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```text
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

## Solution

别脑抽，稍微想一想，一个比较直接的方法，就是将 s  进行分割，用DFS 向下进行check。

其实呢，DFS就是Brute Force的方法，复杂度O\(n^n\)。同时也可以看一看它的**Recursion with memoization,** 虽然我觉得并不是很好的样例，如下：

[https://leetcode.com/problems/word-break/solution/](https://leetcode.com/problems/word-break/solution/)

当然，看了提示之后，发现是DP，再一想，确实也很容易用DP实现。

关键是时间复杂度的分析！！！

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> set = new HashSet<>(wordDict);
        List<Integer> list = new ArrayList<>();
        list.add(-1);
        for (int i = 0; i < s.length(); i++) {
            for (Integer index : list) {
                if (set.contains(s.substring(index + 1, i + 1))) {
                    list.add(i);
                    break;
                }
            }
        }
        return list.contains(s.length() - 1);
    }
}
```




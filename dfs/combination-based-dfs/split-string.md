# Split String

## Description

Give a string, you can choose to split the string after one character or two adjacent characters, and make the string to be composed of only one character or two characters. Output all possible results.

## Example

Given the string `"123"`  
return `[["1","2","3"],["12","3"],["1","23"]]`

## Solution

没啥好说的

```java
public class Solution {
    /*
     * @param : a string to be split
     * @return: all possible split string array
     */
    public List<List<String>> splitString(String s) {
        // write your code here
        List<List<String>> res = new ArrayList<>();
        helper(s, 0, res, new ArrayList<>());
        return res;
    }
    
    public void helper (String s, int index, List<List<String>> res, List<String> cur) {
        if (index >= s.length()) {
            res.add(cur);
            return;
        }
        List<String> choice2 = new ArrayList<>(cur);
        cur.add(s.substring(index, index + 1));
        helper(s, index + 1, res, cur);
        if (index + 2 <= s.length()) {
            choice2.add(s.substring(index, index + 2));
            helper(s, index + 2, res, choice2);
        }
    }
}
```

  



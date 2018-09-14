# Backspace String Compare

## Description

Given two strings `S` and `T`, return if they are equal when both are typed into empty text editors. `#` means a backspace character.

## Example

**Example 1:**

```text
Input: S = "ab#c", T = "ad#c"
Output: true
Explanation: Both S and T become "ac".
```

**Example 2:**

```text
Input: S = "ab##", T = "c#d#"
Output: true
Explanation: Both S and T become "".
```

**Example 3:**

```text
Input: S = "a##c", T = "#a#c"
Output: true
Explanation: Both S and T become "c".
```

**Example 4:**

```text
Input: S = "a#c", T = "b"
Output: false
Explanation: S becomes "c" while T becomes "b".
```

## Follow up

Can you solve it in `O(N)` time and `O(1)` space?

## Solution

首先是比较直白的方法就是用Stack来处理，这里注意一下快速将Stack里面的character组成String的built-in的方法：

妈勒个鸡，注意corner case啊。。

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        String news = buildString(S);
        String newt = buildString(T);
        return news.equals(newt);
    }
    
    private String buildString(String str) {
        Stack<Character> stack = new Stack<>();
        for (Character chr : str.toCharArray()) {
            if (!stack.isEmpty() && chr == '#') {
                stack.pop();
            } else if (chr != '#') {
                stack.push(chr);
            }
        }
        return String.valueOf(stack);
    }
}
```

然后，还有不用Stack的空间复杂度为O\(1\)的两个指针的方法，感觉不难。。不想写了。。两根指针，从后往前扫


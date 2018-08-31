# Basic Calculator II

## Description

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only **non-negative** integers, `+`, `-`, `*`, `/` operators and empty spaces . The integer division should truncate toward zero.

## Example

```text
Input: "3+2*2"
Output: 7
```

```text
Input: " 3/2 "
Output: 1
```

```text
Input: " 3+5 / 2 "
Output: 5
```

## Solution

这个嘛，刚学java的时候，就做过，用两个栈来处理，一个符号栈，一个数字栈。

这是我一开始残破的想法：

```java
class Solution {
    public int calculate(String s) {
        Stack<Integer> numStack = new Stack<>();
        Stack<Character> operatorStack = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            char cur = s.charAt(i);
            if (Character.isDigit(cur)) {
                int num = 0;
                while (Character.isDigit(cur)) {
                    num = num * 10  + (cur - '0')
                    i++;
                    cur = s.charAt(i);
                }
                numStack.put(num);
            } 
        }
    }
}
```

参考了别人的解答，关键在于如何处理数字，只用了一个栈（因为操作简单，没有括号啥的）。

扫描到末尾时的逻辑处理还是出了点问题。

```java
class Solution {
    public int calculate(String s) {
        
        Stack<Integer> stack = new Stack<>();
        int num = 0;
        char sign = '+';
        for (int i = 0; i < s.length(); i++) {
            char cur = s.charAt(i);
            if (Character.isDigit(cur)) {
                num = num * 10 + (cur - '0');
            } 
            
            if (!Character.isDigit(cur) && cur != ' ' || i == s.length() - 1) {
                switch(sign) {
                    case '+':
                        stack.push(num);
                        break;
                    case '-':
                        stack.push(-num);
                        break;
                    case '*':
                        stack.push(stack.pop() * num);
                        break;
                    case '/':
                        stack.push(stack.pop() / num);
                        break;
                }
                sign = cur;
                num = 0;
            }  
        }
        
        int res = 0;
        for (int i : stack) {
            res += i;
        }
        return res;
    }
}
```




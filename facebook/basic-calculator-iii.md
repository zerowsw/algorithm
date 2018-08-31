# Basic Calculator III

## Description

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open `(` and closing parentheses `)`, the plus `+` or minus sign `-`, **non-negative** integers and empty spaces .

The expression string contains only non-negative integers, `+`, `-`, `*`, `/` operators , open `(` and closing parentheses `)` and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of `[-2147483648, 2147483647]`.

## Example

```text
"1 + 1" = 2
" 6-4 / 2 " = 4
"2*(5+5*2)/3+(6/2+8)" = 21
"(2+6* 3+5- (3*14/7+2)*5)+3"=-12
```

## Solution

行了，这道题绝对需要两个栈来处理，一个符号栈，一个数字栈。

之前搞反了，只有在当低优先级的操作符碰到高优先级的操作符的时候，才需要计算一次。\(这个条件需要好好思考一下\)

我还是打算使用这个最正统的方法来实现：

```java
class Solution {
    public int calculate(String s) {
     
        Stack<Character> ops = new Stack<>();
        Stack<Integer> nums = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == ' ') continue;
            if (Character.isDigit(c)) {
                int num = c - '0';
                while (i < s.length() - 1 && Character.isDigit(s.charAt(i + 1))) {
                    num = num * 10 + (s.charAt(i + 1) - '0');
                    i++;
                }
                nums.push(num);
            } else if (c == '(') {
                ops.push(c);
            } else if (c == ')') {
                while (ops.peek() != '(') {
                    nums.push(cal(ops.pop(), nums.pop(), nums.pop()));
                }
                ops.pop();
            } else if (c == '+' || c == '-' || c == '*' || c == '/') {
                if (!ops.isEmpty() && prevOrNot(c, ops.peek())) {
                    nums.push(cal(ops.pop(), nums.pop(), nums.pop()));
                } 
                ops.push(c);
            }
        }
        
        while (!ops.isEmpty()) {
            nums.push(cal(ops.pop(), nums.pop(), nums.pop()));
        }
        return nums.pop();
    }
    
    
    private boolean prevOrNot(char op1, char op2) {
        if (op2 == '(') return false;
        if ((op1 == '*' || op1 == '/') && (op2 == '+' || op2 == '-')) return false;
        return true;
    }
    
    private int cal(char operator, int b, int a) {
        switch(operator){
            case '+':
                    return a + b;
            case '-':
                    return a - b;
            case '*':
                    return a * b;
            case '/':
                    return a / b;
            default:
                    return 0;
        }
    }
}
```




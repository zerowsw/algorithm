---
description: 经典题，练手
---

# Implement Queue by Two Stacks

## Description

As the title described, you should only use two stacks to implement a queue's actions.

The queue should support `push(element)`, `pop()` and `top()` where pop is pop the first\(a.k.a front\) element in the queue.

Both pop and top methods should return the value of first element.

## Example

```text
push(1)
pop()     // return 1
push(2)
push(3)
top()     // return 2
pop()     // return 2
```

## Challenge

implement it by two stacks, do not use any other data structure and push, pop and top should be O\(1\) by _AVERAGE_.

## Solution

没啥好说的

```java
public class MyQueue {
    
    Stack<Integer> stack1;
    Stack<Integer> stack2;
    
    public MyQueue() {
        // do intialization if necessary
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }

    /*
     * @param element: An integer
     * @return: nothing
     */
    public void push(int element) {
        // write your code here
        stack1.push(element);
    }

    /*
     * @return: An integer
     */
    public int pop() {
        // write your code here
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
     }

    /*
     * @return: An integer
     */
    public int top() {
        // write your code here
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.peek();
    }
}
```


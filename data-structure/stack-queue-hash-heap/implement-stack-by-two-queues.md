# Implement Stack by Two Queues

## Description

Implement a stack by two queues. The queue is first in first out \(FIFO\). That means you can not directly pop the last element in a queue.

## Example

```text
push(1)
pop()
push(2)
isEmpty() // return false
top() // return 2
pop()
isEmpty() // return true
```

## Solution

傻了吧。。

还真是这样的做法，为了获取top元素真的需要倾倒一个队列直到获取它的尾部元素。

我没想到的是，如何有效的switch从而不会搞混两个队列。（直接交换就可以了）

这里犯了一个低级错误：在函数swap\(Queue q1,  Queue q2\) 中交换q1 和 q2 并不会交换原本的queue1 和queue2, 我交换的是函数内部复制的局部引用变量。

```java
public class Stack {
    /*
     * @param x: An integer
     * @return: nothing
     */
    
    Queue<Integer> queue1 = new LinkedList<>();
    Queue<Integer> queue2 = new LinkedList<>();
    
    public void push(int x) {
        // write your code here
        queue1.offer(x);
        moveElement();
    }

    /*
     * @return: nothing
     */
    public void pop() {
        // write your code here
        if (queue1.isEmpty()) {
            swap();
        }
        moveElement();
        queue1.poll();
    }
    
    public void swap() {
        Queue<Integer> temp = queue2;
        queue2 = queue1;
        queue1 = temp;
    }

    public void moveElement() {
        while (queue1.size() > 1) {
            queue2.offer(queue1.poll());
        }
    }
    /*
     * @return: An integer
     */
    public int top() {
        // write your code here
        if (queue1.isEmpty()) {
            swap();
        }
        moveElement();
        return queue1.peek();
    }

    /*
     * @return: True if the stack is empty
     */
    public boolean isEmpty() {
        // write your code here
        return queue1.isEmpty() && queue2.isEmpty();
    }
}
```


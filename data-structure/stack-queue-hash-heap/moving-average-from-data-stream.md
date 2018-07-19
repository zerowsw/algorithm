# Moving Average from Data Stream

## Description

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

## Example

```text
MovingAverage m = new MovingAverage(3);
m.next(1) = 1 // return 1.00000
m.next(10) = (1 + 10) / 2 // return 5.50000
m.next(3) = (1 + 10 + 3) / 3 // return 4.66667
m.next(5) = (10 + 3 + 5) / 3 // return 6.00000
```

## Solution

不要想当然地就用几个变量，需要存储Windows数量的数字。

然而，没跑过所有的test case， 即便考虑了overflow，也还是没过：

```java
public class MovingAverage {
    
    int size = 0;
    Queue<Integer> queue;
    double average = 0.0; 
    /*
    * @param size: An integer
    */public MovingAverage(int size) {
        // do intialization if necessary
        this.size = size;
        queue = new LinkedList<>();
    }

    /*
     * @param val: An integer
     * @return:  
     */
    public double next(int val) {
        // write your code here
        if (queue.size() == size) {
            int old = queue.poll();
            queue.offer(val);
            //average = (average * size - old + val) / size;
            //to avoid overflow
            average = average + (double)(val - old) / size;
        } else {
            //average = (queue.size() * average + val) / (queue.size() + 1);
            //to avoid overflow
            average = average * ((double)queue.size() / (queue.size() + 1)) + (double)val / (queue.size() + 1);
            
            queue.offer(val);
        }
        return average;
    }
}
```

我去，你是不是傻，，整数相除的得到的结果，保存成double（精度损失），再乘以size，并不能还原。。。。。。。。

用double存sum即可，并不会溢出。。

```text
public class MovingAverage {
    
    int size = 0;
    Queue<Integer> queue;
    double sum = 0.0;
    /*
    * @param size: An integer
    */public MovingAverage(int size) {
        // do intialization if necessary
        this.size = size;
        queue = new LinkedList<>();
    }

    /*
     * @param val: An integer
     * @return:  
     */
    public double next(int val) {
        // write your code here
        if (queue.size() == size) {
            int old = queue.poll();
            queue.offer(val);
            sum = sum + val - old;
        } else {
            sum = sum + val;
            queue.offer(val);
        }
        return sum / queue.size();
    }
}
```


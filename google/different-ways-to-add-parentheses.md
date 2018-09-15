# Different Ways to Add Parentheses

## Description

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are `+`, `-` and `*`.

## Example

**Example 1:**

```text
Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2
```

**Example 2:**

```text
Input: "2*3-4*5"
Output: [-34, -14, -10, -10, 10]
Explanation: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```

## Solution

他偏偏抽了这道比较直白的题目。。

我的解法：

```java
class Solution {
    
    public List<Integer> diffWaysToCompute(String input) {

        List<Integer> result = new ArrayList<Integer>();
        //boolean numOrNot = true;
        for (int i = 0; i < input.length(); i++) {
                char cur = input.charAt(i);
                if (cur == '-' || cur == '+' || cur == '*') {
                    //numOrNot = false;
                    List<Integer> left = diffWaysToCompute(input.substring(0, i));
                    List<Integer> right = diffWaysToCompute(input.substring(i + 1, input.length()));
                    result.addAll(calculate(cur, left, right));
                }
        }

        if (result.size() == 0) {
            result.add(Integer.parseInt(input));
        }
        return result;
    }
    
    private List<Integer> calculate(char operator, List<Integer> a, List<Integer> b) {
		List<Integer> result = new ArrayList<Integer>();
		for (Integer i : a) {
			for (Integer j : b) {
                switch(operator) {
                    case '+' :
                        result.add(i + j);
                        break;
                    case '-' :
                        result.add(i - j);
                        break;
                    case '*' :
                        result.add(i * j);
                        break;
                }
            }
        }
        return result;
    }
}
```


# Valid Number

## Description

原题描述：

Validate if a given string can be interpreted as a decimal number.

Some examples:  
`"0"` =&gt; `true`  
`" 0.1 "` =&gt; `true`  
`"abc"` =&gt; `false`  
`"1 a"` =&gt; `false`  
`"2e10"` =&gt; `true`  
`" -90e3   "` =&gt; `true`  
`" 1e"` =&gt; `false`  
`"e3"` =&gt; `false`  
`" 6e-1"` =&gt; `true`  
`" 99e2.5 "` =&gt; `false`  
`"53.5e93"` =&gt; `true`  
`" --6 "` =&gt; `false`  
`"-+3"` =&gt; `false`  
`"95a54e53"` =&gt; `false`

**Note:** It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one. However, here is a list of characters that can be in a valid decimal number:

* Numbers 0-9
* Exponent - "e"
* Positive/negative sign - "+"/"-"
* Decimal point - "."

Of course, the context of these characters also matters in the input.

**Update \(2015-02-10\):**  
The signature of the `C++` function had been updated. If you still see your function signature accepts a `const char *` argument, please click the reload button to reset your code definition.

面试中出现的简化版本：

  
**判断一个string是不是一个valid的数（整数小数负数）**

**"-12" -&gt;true**

**"+12" -&gt; false**

**"-12.23" -&gt;true**

**"12ab" -&gt; false**

**"12.23.1"-&gt;false** 

**具体哪个算valid可以自己定，和面试官探讨就好。总的就是先看前面是不是负号，然后中间有没有小数点，有的话只能有一个，而且小数点前必须有一个数字，后面也至少要有一个。不难想，就是写起来很繁琐。**

```java
private boolean validNumber(String number) {
        if (number == null || number.length() == 0) return false;
        int i = 0;
        boolean finddot = false;
        while (i < number.length()) {
            char cur = number.charAt(i);
            if (i == 0) {
                if (!Character.isDigit(cur) && cur != '-') return false;
                //下面两个if保证了开头不能是0，或者说只能是0.XXX的形式
                if (cur == '0' && number.length() > 1) return false;
                if (cur == '-' && (number.length() == 1 || number.length() > 2 && number.charAt(1) == '0' && number.charAt(2) != '.')) return false;
            } else if (cur == '.') {
                if (finddot) return false;
                if (!Character.isDigit(number.charAt(i - 1))) return false;
                if (i == number.length() - 1) return false;
                finddot = true;
            } else if (!Character.isDigit(cur)) {
                return false;
            }
            i++;
        }
        return true;
    }
```

\*\*\*\*


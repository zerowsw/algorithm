# Validate IP Address

## Description

Write a function to check whether an input string is a valid IPv4 address or IPv6 address or neither.

**IPv4** addresses are canonically represented in dot-decimal notation, which consists of four decimal numbers, each ranging from 0 to 255, separated by dots \("."\), e.g.,`172.16.254.1`;

Besides, leading zeros in the IPv4 is invalid. For example, the address `172.16.254.01` is invalid.

**IPv6** addresses are represented as eight groups of four hexadecimal digits, each group representing 16 bits. The groups are separated by colons \(":"\). For example, the address `2001:0db8:85a3:0000:0000:8a2e:0370:7334` is a valid one. Also, we could omit some leading zeros among four hexadecimal digits and some low-case characters in the address to upper-case ones, so `2001:db8:85a3:0:0:8A2E:0370:7334` is also a valid IPv6 address\(Omit leading zeros and using upper cases\).

However, we don't replace a consecutive group of zero value with a single empty group using two consecutive colons \(::\) to pursue simplicity. For example, `2001:0db8:85a3::8A2E:0370:7334` is an invalid IPv6 address.

Besides, extra leading zeros in the IPv6 is also invalid. For example, the address `02001:0db8:85a3:0000:0000:8a2e:0370:7334` is invalid.

**Note:** You may assume there is no extra space or special characters in the input string.

## Example

**Example 1:**  


```text
Input: "172.16.254.1"

Output: "IPv4"

Explanation: This is a valid IPv4 address, return "IPv4".
```

**Example 2:**  


```text
Input: "2001:0db8:85a3:0:0:8A2E:0370:7334"

Output: "IPv6"

Explanation: This is a valid IPv6 address, return "IPv6".
```

**Example 3:**  


```text
Input: "256.256.256.256"

Output: "Neither"

Explanation: This is neither a IPv4 address nor a IPv6 address.
```

## Solution

"2001:0db8:85a3:0:0:8A2E:0370:7334:" 这种东西split之后

我他么。。。呵呵哒了。。

```java
class Solution {
    public String validIPAddress(String IP) {
        if (IP == null || IP.length() == 0) return "Neither";
        if (IP.charAt(0) == '.' || IP.charAt(0) == ':' || IP.charAt(IP.length() - 1) == '.' || IP.charAt(IP.length() - 1) == ':') return "Neither";
        String[] ipv4 = IP.split("\\.");
        String[] ipv6 = IP.split(":");
        if (ipv4.length == 4) {
            return checkIPv4(ipv4) ? "IPv4" : "Neither";
        } else if (ipv6.length == 8) {
            return checkIPv6(ipv6) ? "IPv6" : "Neither";
        } else {
            return "Neither";
        }
    }
    
    private boolean checkIPv6(String[] ipv6) {
        for (int i = 0; i < 8; i++) {
            String cur = ipv6[i];
            if (cur == null || cur.length() == 0) return false;
            if (cur.length() > 4) return false;
            for (int j = 0; j < cur.length(); j++) {
                if (!checkChar(cur.charAt(j))) return false;
            }
        }
        return true;
    }
    
    private boolean checkChar(char c) {
        if (Character.isDigit(c) || ( c >= 'a' && c <= 'f') || (c >= 'A' && c <= 'F')) return true;
        return false;
    }
    
    private boolean checkIPv4(String[] ipv4) {
        for (int i = 0; i < 4; i++) {
            String cur = ipv4[i];
            if (cur == null || cur.length() == 0 || cur.length() > 3) return false;
            if (cur.charAt(0) == '0' && cur.length() > 1) return false;
            int num = 0;
            for (int j = 0; j < cur.length(); j++) {
                if (!Character.isDigit(cur.charAt(j))) return false;
                num = num * 10 + (cur.charAt(j) - '0');
            }
            //你他么能这样判断？？不怕overflow？你脑子装的是屎吧。。
            if (num > 255) return false;
        }
        return true;
    }
}
```


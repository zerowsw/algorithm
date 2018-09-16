# First Bad Version

## Description

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

## Example

```text
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version. 
```

## Solution

注意指针的移动

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int le = 1;
        int ri = n;
        while (le < ri) {
            int mid = le + (ri - le) / 2;
            if (isBadVersion(mid)) {
                ri = mid;
            } else {
                le = mid + 1;
            }
        }
        return isBadVersion(ri) ? ri : le; 
    }
}
```

这里换一个解法，用一个res来记录，这样的话，指针移动就比较一致了：

```text
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int le = 1;
        int ri = n;
        int res = -1;
        while (le <= ri) {
            int mid = le + (ri - le) / 2;
            if (isBadVersion(mid)) {
                ri = mid - 1;
                res = mid;
            } else {
                le = mid + 1;
            }
        }
        return isBadVersion(ri) ? ri : le; 
    }
}
```


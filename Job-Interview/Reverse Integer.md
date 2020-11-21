### Link
---
https://leetcode.com/problems/reverse-integer/

<br>

### Solution
---

<br>

```java
class Solution {

    public int reverse(int x) {

        long ret = 0;
        while(x != 0){
            int num = x % 10;
            x = x / 10;
            ret = ret * 10 + num;
        }

        if(ret < Integer.MIN_VALUE || Integer.MAX_VALUE < ret) return 0;

        return (int)ret;
    }

}
```

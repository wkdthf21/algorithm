### Link
---
https://leetcode.com/problems/multiply-strings/

<br>


### Solution
---

```java
class Solution {
    public String multiply(String num1, String num2) {

        if(num1.charAt(0) - '0' == 0 || num2.charAt(0) - '0' == 0) return "0";

        int lenNum1 = num1.length();
        int lenNum2 = num2.length();
        int[] result = new int[lenNum1 + lenNum2];
        int[] n1 = new int[lenNum1];
        int[] n2 = new int[lenNum2];
        String answer = "";

        for(int i = lenNum1 - 1; i >= 0; i--) n1[lenNum1 - 1 - i] = num1.charAt(i) - '0';
        for(int i = lenNum2 - 1; i >= 0; i--) n2[lenNum2 - 1 - i] = num2.charAt(i) - '0';

        for(int i = 0; i < lenNum1; i++){
            for(int j = 0; j < lenNum2; j++){
                result[i+j] += n1[i] * n2[j];
            }
        }

        int i = 0;
        for(i = 0; i < result.length - 1; i++){
            result[i+1] += result[i] / 10;
            result[i] = result[i] % 10;
            answer = Integer.toString(result[i]) + answer;
        }

        if(result[i] != 0) answer = Integer.toString(result[i]) + answer;

        return answer;

    }
}
```

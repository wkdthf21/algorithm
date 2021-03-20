### Link
---
https://leetcode.com/problems/roman-to-integer/

<br>


### Solution
---
```java
import java.util.*;

class Solution {

    public int romanToInt(String s) {

        int answer = 0;

        Map<Character, Integer> symbolMap = new HashMap<>();
        symbolMap.put('I', 1);
        symbolMap.put('V', 5);
        symbolMap.put('X', 10);
        symbolMap.put('L', 50);
        symbolMap.put('C', 100);
        symbolMap.put('D', 500);
        symbolMap.put('M', 1000);

        for(int i = 0; i < s.length(); i++){
            if(i != s.length() - 1 && symbolMap.get(s.charAt(i)) < symbolMap.get(s.charAt(i+1))){
                answer = answer - symbolMap.get(s.charAt(i));
            }
            else{
                answer = answer + symbolMap.get(s.charAt(i));
            }
        }

        return answer;
    }
}
```

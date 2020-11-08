### Link
---
https://leetcode.com/problems/longest-substring-without-repeating-characters

<br>

### Solution
---

- map에 Character를 Key로 하여 index를 Value로 저장합니다.
- map에 Character가 존재할 경우 저장된 Value와 start중 큰 쪽을 start로 지정합니다.
- 매번 answer를 업데이트 해줍니다.

<br>


```java
import java.util.*;

class Solution {
    public int lengthOfLongestSubstring(String s) {

        Map<Character, Integer> map = new HashMap<>();
        int answer = 0, len = 0;
        for(int start = 0, end = 0; end < s.length(); end++){
            char c = s.charAt(end);
            if(map.containsKey(c)){
                start = start < map.get(c) + 1 ? map.get(c) + 1 : start;    
            }

            len = end - start + 1;
            answer = answer < len ? len : answer;
            map.put(c, end);
        }


        return answer;

    }
}
```

### Link
---
https://programmers.co.kr/learn/courses/30/lessons/42747

<br>


### 풀이 1
---

- 단순하게 생각했을 때
- 숫자 0부터 최대 인용된 횟수만큼 반복하면서
- 조건을 만족하는지 확인

<br>
```java
import java.util.*;

class Solution {
    public int solution(int[] citations) {
        int answer = 0;

        Arrays.sort(citations);

        int length = citations.length;
        int idx = 0;
        for(int curNum = 0; curNum < citations[length-1]; curNum++){
            for(; idx < length; idx++){
                if(curNum <= citations[idx]){
                    break;
                }
            }

            if(length - idx >= curNum && idx <= curNum){
                if(answer < curNum) answer = curNum;
            }
        }

        return answer;
    }

}
```
<br>

### 풀이 2
---

- 뒤에서부터 보면서
- 원소 값은 점점 감소하고, 그 해당 원소 값 이상인 것의 개수는 증가하는데
- 둘 중 최소값의 최대값이 정답이 된다

<br>

```java
import java.util.*;

class Solution {
    public int solution(int[] citations) {
        Arrays.sort(citations);

        int max = 0;
        for(int i = citations.length-1; i > -1; i--){
            int min = (int)Math.min(citations[i], citations.length - i);
            if(max < min) max = min;
        }

        return max;
    }
}
```


<br>

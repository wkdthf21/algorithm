### Link
---
https://leetcode.com/problems/zigzag-conversion/
<br>


### Solution
---

- 문자열을 지그재그로 나열할 때
- 가장 첫번째 줄에 있는 인덱스들의 배수를 topPoint로 잡았다
- topPoint에서 i = 0부터 i = numRows - 1까지 빼고 더해주면 정답 문자열을 만들 수 있다
- topPoint는 각 i마다 0부터 topPoint씩 증가하는데
  - 매번 증가한 결과값을 j에 저장한다
  - 이때 j에서 i를 더한 값이 문자열 길이를 넘는지 체크한다
  - 체크 후 j + i 번째 인덱스를 넣고
  - i = 0과 i = numRows-1 가 아니면서, j가 topPoint만큼 증가한거에 - i를 한 값이 문자열 길이를 넘지 않는다면, 넣어준다

- 문자열의 인덱스를 1번만 방문하므로 시간 복잡도는 O(문자열 길이)

<br>
```java
class Solution {
    public String convert(String s, int numRows) {

        if(numRows == 1) return s;

        StringBuilder sb = new StringBuilder();
        int topPoint = 2 * numRows - 2;
        for(int i = 0; i < numRows; i++){
            for(int j = 0; j + i < s.length(); j += topPoint){
                sb.append(s.charAt(j + i));
                int nTopPrev = j + topPoint - i;
                if(i != 0 && i != numRows - 1 && nTopPrev < s.length()){
                    sb.append(s.charAt(nTopPrev));
                }
            }
        }

        return sb.toString();

    }
}
```

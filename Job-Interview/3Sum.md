### Link
---
https://leetcode.com/explore/interview/card/top-interview-questions-medium/103/array-and-strings/776/

<br>

### Explanation
---

- nums를 순회하면서 숫자 1개 선택 - num1
- 0이 기준이지만 num 중 한 개를 선택했으므로 그 숫자를 기준으로 나머지 리스트에서 합이 -num1이 되는 수를 찾는다 - num2
  - map을 이용한다
  - 만약 num1이 - 3이고(합이 3인 수를 찾아야 한다) num2가 1인데 map에 없다면 map에는 3 - 1 = 2을 넣어둔다
  - 추후 2가 num2가 될 때 map에 존재하므로 -3, 1, 2 조합을 넣어준다
  - 넣어줄 때 중복 체크를 위해 String이 원소인 Set을 이용했다.


<br>

```java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {

        List<List<Integer>> answer = new ArrayList<>();
        Arrays.sort(nums);
        Map<Integer, Integer> map = new HashMap<>();
        Set<String> set = new HashSet<>();

        for(int i = 0; i < nums.length; i++){
            int num = nums[i];
            int target = -num;
            map.clear();
            for(int j = i + 1; j <nums.length ; j++){
                if(map.containsKey(nums[j])){
                    String s = Integer.toString(num) + Integer.toString(target - nums[j]) + Integer.toString(nums[j]);
                    if(set.contains(s) == false){
                        List<Integer> list = new ArrayList<>();
                        list.add(num); list.add(nums[j]); list.add(target - nums[j]);
                        answer.add(list);
                        set.add(s);
                    }

                }
                map.put(target - nums[j], j);

            }
        }

        return answer;

    }
}
```

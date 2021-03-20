### Link
---
https://www.hackerrank.com/challenges/frequency-queries/problem?h_l=interview&playlist_slugs%5B%5D%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D%5B%5D=dictionaries-hashmaps

<br>

### Explanation
---

- 숫자들을 추가하고 삭제하는데
- 쿼리 3은 frequency가 z인 숫자가 몇 개인지 return 해야한다.
- 이를 위해 숫자 각각의 count를 저장하는 numMap과
- count별 숫자 개수를 저장하는 countMap을 두었다. 


<br>

```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;

public class Solution {

    // Complete the freqQuery function below.
    static List<Integer> freqQuery(List<List<Integer>> queries) {

        List<Integer> ansList = new ArrayList<>();
        Map<Integer, Integer> numMap = new TreeMap<>();
        Map<Integer, Integer> countMap = new TreeMap<>();

        for(List<Integer> query : queries){
            int q = query.get(0);
            int n = query.get(1);

            if(q == 1){

                int numCnt = 1;
                // num, numCount
                if(numMap.containsKey(n) && numMap.get(n) != 0) {
                    numCnt = numMap.get(n) + 1;
                    countMap.put(numCnt - 1, countMap.get(numCnt - 1) - 1);
                    numMap.put(n, numCnt);
                }
                else{
                    numMap.put(n, 1);
                }

                // count, count of count
                if(countMap.containsKey(numCnt)){
                    countMap.put(numCnt, countMap.get(numCnt) + 1);
                }
                else{
                    countMap.put(numCnt, 1);
                }

            }
            else if(q == 2){
                if(numMap.containsKey(n) && numMap.get(n) != 0){
                    int numCnt = numMap.get(n);
                    countMap.put(numCnt, countMap.get(numCnt) - 1);
                    numMap.put(n, numCnt - 1);

                    if(numCnt - 1 == 0) continue;
                    if(countMap.containsKey(numCnt - 1)) {
                        countMap.put(numCnt - 1, countMap.get(numCnt - 1) + 1);
                    }
                    else countMap.put(numCnt - 1, 1);
                }
            }
            else if(q == 3){

                if(countMap.containsKey(n) && countMap.get(n) != 0){
                    ansList.add(1);
                }
                else ansList.add(0);

            }

        }

        return ansList;

    }

    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int q = Integer.parseInt(bufferedReader.readLine().trim());

        List<List<Integer>> queries = new ArrayList<>();

        IntStream.range(0, q).forEach(i -> {
            try {
                queries.add(
                    Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                        .map(Integer::parseInt)
                        .collect(toList())
                );
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        List<Integer> ans = freqQuery(queries);

        bufferedWriter.write(
            ans.stream()
                .map(Object::toString)
                .collect(joining("\n"))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}

```

### Link
---
https://leetcode.com/problems/integer-to-roman/

<br>


### Solution 1
---
- 가능한 Roman을 선언해두는 경우
  - 900, 400 등 2가지 문자를 필요로 하는 경우도 모두 선언
- Runtime: **4 ms**, faster than **84.27%** of Java online submissions for Integer to Roman.
- Memory Usage: **38.2 MB**, less than **90.13%** of Java online submissions for Integer to Roman.

```java
import java.util.*;

class Solution {

    int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    public String intToRoman(int num) {

        StringBuilder answer = new StringBuilder();

        for(int i = 0; i < values.length; i++) {
            while(num >= values[i]) {
                num -= values[i];
                answer.append(symbols[i]);
            }
        }

        return answer.toString();

    }


}
```

<br>

### Solution 2
---

- 선언해두지 않는 경우
  - 앞자리가 4나 9인 경우는 2가지 문자를 필요로 하는 경우로 처리
  - 그 외의 경우인 5로 시작하는 숫자나 1로 시작하는 숫자로 나눠야되는 경우 처리

  - Runtime: **7 ms**, faster than **38.40%** of Java online submissions for Integer to Roman.
  - Memory Usage: **38.5 MB**, less than **76.53%** of Java online submissions for Integer to Roman.

```java
import java.util.*;

class Solution {

    Map<Integer, String> valueToSymbol = new HashMap<>();

    public String intToRoman(int num) {

        valueToSymbol.put(1, "I");
        valueToSymbol.put(5, "V");
        valueToSymbol.put(10, "X");
        valueToSymbol.put(50, "L");
        valueToSymbol.put(100, "C");
        valueToSymbol.put(500, "D");
        valueToSymbol.put(1000, "M");

        StringBuilder answer = new StringBuilder();
        int numSize;

        while(num != 0){

            numSize = (int)Math.log10(num) + 1;
            int n10 = (int)Math.pow(10, numSize - 1);
            int n5 = (int)Math.pow(10, numSize - 1) * 5;
            int n, q, r;

            q = num / n10;
            r = num % n10;

            if(q == 4 || q == 9){
                intToTwoCharacterRoman(num, numSize, q, r, answer);
            }
            else if(n5 <= num && Math.abs(num - n5) < Math.abs(num - n10)){
                q = num / n5;
                r = num % n5;
                for(int i = 0; i < q; i++){
                    answer.append(valueToSymbol.get(n5));
                }
            }
            else{
                for(int i = 0; i < q; i++){
                    answer.append(valueToSymbol.get(n10));
                }
            }

            --numSize;
            num = r;
        }

        return answer.toString();

    }


    public void intToTwoCharacterRoman(int num, int numSize, int q, int r, StringBuilder sb){

        if(q == 4){
            int n1 = (int)Math.pow(10, numSize - 1) * 5;
            int n2 = (int)Math.pow(10, numSize - 1);
            sb.append(valueToSymbol.get(n2));
            sb.append(valueToSymbol.get(n1));
            r = num % (n1 - n2);
        }
        else if(q == 9){
            int n1 = (int)Math.pow(10, numSize);
            int n2 = (int)Math.pow(10, numSize - 1);
            sb.append(valueToSymbol.get(n2));
            sb.append(valueToSymbol.get(n1));
            r = num % (n1 - n2);
        }

    }

}
```

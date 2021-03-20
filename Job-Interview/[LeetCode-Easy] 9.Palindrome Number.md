### Link
---
https://leetcode.com/problems/palindrome-number/

<br>

### Solution
---
- 숫자를 문자형으로 바꾸지 않고 palindrome인지 확인

<br>

- Solution 1

  - 숫자를 10으로 나눈 나머지를 이용해서 거꾸로 된 숫자를 만들고 비교한다

  ```java
  import java.util.*;

  class Solution {
      public boolean isPalindrome(int x) {

          // 음수거나
          // 0이 아닌 0으로 끝나는 양수들은
          // 전부 Palindrome이 될 수 없다
          if(x < 0 || (x % 10 == 0 && x != 0)) return false;

          int reversedNum = 0;
          int num = x;
          while(num != 0){
              reversedNum = reversedNum * 10 + num % 10;
              num = num / 10;
          }

          return reversedNum == x;
      }
  }
  ```

<br>

- Solution 2

  - Solution 1에서 조금만 더 생각해보면
  - 절반까지만 비교해보면 된다
  - 자리수가 짝수인 수의 경우 절반까지만 reversedNum을 만들고 비교하면 되고
  - 홀수인 경우 reversedNum의 자릿수가 1개 더 커지게 되는데
  - reversedNum의 일의 자리는 전체 수의 가운데 숫자라 필요없다
  - 따라서 reversedNum / 10의 값이 x와 같은지 비교해보면 된다

  <br>

  ```java
  import java.util.*;

  class Solution {
      public boolean isPalindrome(int x) {

          // 음수거나
          // 0이 아닌 0으로 끝나는 양수들은
          // 전부 Palindrome이 될 수 없다
          if(x < 0 || (x % 10 == 0 && x != 0)) return false;

          int reversedNum = 0;
          while(x > reversedNum){
              reversedNum = reversedNum * 10 + x % 10;
              x = x / 10;
          }

          return x == reversedNum || x == reversedNum / 10;
      }
  }
  ```

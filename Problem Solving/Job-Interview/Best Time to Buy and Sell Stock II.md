### 문제 링크
---
https://leetcode.com/explore/interview/card/top-interview-questions-easy/92/array/564/

<br>

### 풀이
---

1. O(n^2)

```java
class Solution {

    public int maxProfit(int[] prices) {

        int[] dp = new int[prices.length];
        int maxCost = 0;

        Arrays.fill(dp, 0);

        for(int i = 0; i < prices.length; i++){
            for(int j = i + 1; j < prices.length; j++){
                int diff = prices[j] - prices[i];
                if(prices[i] < prices[j]){
                    dp[j] = (dp[j] < dp[i] + diff) ? dp[i] + diff : dp[j];
                    maxCost = maxCost < dp[j] ? dp[j] : maxCost;
                }
                else if(prices[j] < prices[i]){
                    dp[j] = (dp[j] < dp[i]) ? dp[i] : dp[j];
                    maxCost = maxCost < dp[j] ? dp[j] : maxCost;
                }
            }
        }

        return maxCost;

    }
}
```

<br>


2. O(n)

- 최대한 인접하면서 이득이 많은 경우 팔기
- 인접한 요소끼리 가격 비교 하면서 buyIdx에는 최저값이 sellIdx에는 최대값이

```java

class Solution {

    public int maxProfit(int[] prices) {


        int maxCost = 0;
        int i = 0;
        int buyIdx = 0;
        int sellIdx = 0;

        while(i < prices.length - 1){

            // buy
            while(i < prices.length - 1 && prices[i + 1] <= prices[i]) ++i;

            buyIdx = i;

            // sell
            while(i < prices.length - 1 && prices[i] <= prices[i+1]) ++i;

            sellIdx = i;

            maxCost += prices[sellIdx] - prices[buyIdx];
        }


        return maxCost;

    }
}

```

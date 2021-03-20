### Link
---
https://leetcode.com/explore/interview/card/top-interview-questions-medium/103/array-and-strings/777/

<br>

### Solution
---

1. Row-Set, Column-Set
  - Time Complexity : O(MN)
  - Space Complexity : O(M + N)

<br>

```java
import java.util.*;

class Solution {

    public void setZeroes(int[][] matrix) {


        Set<Integer> rowSet = new HashSet<>();
        Set<Integer> colSet = new HashSet<>();


        for(int i = 0; i < matrix.length; i++){

            for(int j = 0; j < matrix[i].length; j++){

                if(matrix[i][j] == 0){
                    rowSet.add(i);
                    colSet.add(j);
                }

            }

        }


        for(int i = 0; i < matrix.length; i++){

            for(int j = 0; j < matrix[i].length; j++){

                if(rowSet.contains(i) || colSet.contains(j)){
                    matrix[i][j] = 0;
                }

            }

        }


    }
}

```

<br>

2. Using 0 row, col as Flag
  - Time Complexity : O(MN)
  - Space Complexity : O(1)


<br>

``` java
import java.util.*;

class Solution {

    public void setZeroes(int[][] matrix) {

        Boolean isFirstColZero = false;
        int R = matrix.length;
        int C = matrix[0].length;

        for(int i = 0; i < R; i++){

            if(matrix[i][0] == 0) isFirstColZero = true;

            for(int j = 1; j < C; j++){

                if(matrix[i][j] == 0){
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }

            }

        }


         for(int i = 1; i < R; i++){

            for(int j = 1; j < C; j++){

                if(matrix[0][j] == 0 || matrix[i][0] == 0){
                    matrix[i][j] = 0;
                }

            }

        }

        if(matrix[0][0] == 0){
            for(int i = 0; i < C; i++) matrix[0][i] = 0;
        }


        if(isFirstColZero){

            for(int i = 0; i < R; i++){
                matrix[i][0] = 0;
            }


        }

    }
}
```

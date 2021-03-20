### Link
---
https://leetcode.com/problems/generate-parentheses/

<br>

### Solution
---

1. backtracking

```java
import java.util.*;

class Solution {

    List<String> list = new ArrayList<>();
    private void dfs(int openCnt, int closeCnt, StringBuilder sb){

        if(openCnt == 0 && closeCnt == 0){
            list.add(sb.toString());
            return;
        }

        if(0 < openCnt){
            sb.append("(");
            dfs(openCnt - 1, closeCnt, sb);
            sb.deleteCharAt(sb.length() - 1);
        }
        if(openCnt < closeCnt){
            sb.append(")");
            dfs(openCnt, closeCnt-1, sb);
            sb.deleteCharAt(sb.length() - 1);
        }

    }



    public List<String> generateParenthesis(int n) {
        StringBuilder sb = new StringBuilder();
        dfs(n, n, sb);
        return list;
    }


}
```

<br>

2. Close Number

```java
import java.util.*;

class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList();
        if (n == 0) {
            ans.add("");
        } else {
            for (int c = 0; c < n; ++c)
                for (String left: generateParenthesis(c))
                    for (String right: generateParenthesis(n-1-c))
                        ans.add("(" + left + ")" + right);
        }
        return ans;
    }
}

```

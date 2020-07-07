### 풀이
***
#### 주의할 점

1. 처음에 앞에 숫자가 뒤에 숫자보다 클 때만 바꾸도록 했으나,
주어진 횟수만큼 무조건 바꿔야 되기 때문에
<br>

    > 1
    94 1

과 같은 케이스의 경우 에러가 난다.

2. visited 배열을 쓰지 않으면 시간초과가 걸린다.
똑같은 상금에 똑같은 교환 횟수를 탐색한 적이 있는 경우는 더이상 탐색하지 않는다.


```java
#include<iostream>
#include<algorithm>
#include<memory.h>

using namespace std;

int swapCount, maxAns, len;
char num[7];
bool visited[1000000][11];  // 상금 / 교환횟수 배열

void dfs(int count){


    if(count == swapCount){
        int total = atoi(num);
        maxAns = maxAns < total ? total : maxAns;
        return;
    }


    for(int i = 0; i < len; i++){
        for(int j = i+1; j < len; j++){
            swap(num[i], num[j]);
            if(!visited[atoi(num)][count+1]){   // 숫자를 바꿨을때, 같은 교환 횟수로 같은 상금이 나온적이 없을때만 진행
                visited[atoi(num)][count+1] = true;
                dfs(count+1);
            }
            swap(num[i], num[j]);
        }
    }
}


int main(int argc, char** argv)
{
    int test_case;
    int T;

    scanf("%d", &T);

    for(test_case = 1; test_case <= T; ++test_case)
    {   
        scanf("%s %d", num, &swapCount);

        // initialize
        maxAns = 0;
        len = strlen(num);
        memset(visited, false, sizeof(visited));;

        // dfs
        dfs(0);

        printf("#%d %d \n", test_case, maxAns);

    }

    return 0;//정상종료시 반드시 0을 리턴해야합니다.
}
```

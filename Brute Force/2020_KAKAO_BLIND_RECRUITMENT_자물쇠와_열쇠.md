2020_KAKAO_BLIND_RECRUITMENT : 자물쇠와 열쇠
===========
### 문제 설명
---
고고학자인 튜브는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 자물쇠로 잠겨 있었고 문 앞에는 특이한 형태의 열쇠와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가 1 x 1인 N x N 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 M x M 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

<br>

### 제한사항
---
key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
M은 항상 N 이하입니다.
key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
0은 홈 부분, 1은 돌기 부분을 나타냅니다.

<br>

### 풀이

1. 회전은 최대 시계방향으로 4번까지 밖에 안된다. (반시계 방향 == 시계방향 3번)

2. lock 을 바닥에 두고 key를 이동시킨다.
  - key의 모든 y, x 좌표에 -n 부터 ~ n 까지 더한다
  - 더한 좌표는 lock의 좌표로 사용한다
  - lock의 좌표가 유효하지 않으면 lock과 key가 겹쳐진 부분이 아닌 좌표이므로 넘어간다.
  - key의 좌표와 lock의 좌표를 비교하여 모든 홈을 다 채웠는지 / 돌기가 일치하는 부분이 없는지 체크해주면 된다.
  - 이해가 안가면 그림을 그려보자.


```c++
#include <string>
#include <vector>
using namespace std;

void rotate(vector<vector<int>> &key){

    int m = key.size();
    vector<vector<int>> temp(m, vector<int>(m, 0));

    for(int i = 0; i < m; i++){
        for(int j = 0; j < m; j++){
            temp[i][j] = key[j][m-1-i];
        }
    }

    key = temp;
}

bool solution(vector<vector<int>> key, vector<vector<int>> lock) {

    int m = key.size();
    int n = lock.size();
    int hom = 0;

    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            if(lock[i][j] == 0) ++hom;
        }
    }

    for(int r = 0; r < 4; r++){
        rotate(key);
        for(int i = -n; i <= n; i++){
            for(int j = -n; j <= n; j++){
                int cnt = 0; bool isSuccess = true;
                for(int y = 0; y < m; y++){
                    for(int x = 0; x < m; x++){
                        int ny = y + i;
                        int nx = x + j;
                        if(ny < 0 || ny >= n || nx < 0 || nx >= n) continue;
                        // 흠이 일치
                        if(key[y][x] == 1 && lock[ny][nx] == 0) ++cnt;
                        // 돌기가 일치
                        else if(key[y][x] == 1 && lock[ny][nx] == 1) isSuccess = false;

                    }
                }
                if(cnt == hom && isSuccess) return true;
            }
        }
    }
    return false;
}
```

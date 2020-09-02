2020 KAKAO BLIND RECRUITMENT : 블록 이동하기
===
### 문제 링크
---
https://programmers.co.kr/learn/courses/30/lessons/60063
<br>

### 문제 설명
---
로봇개발자 무지는 한 달 앞으로 다가온 카카오배 로봇경진대회에 출품할 로봇을 준비하고 있습니다. 준비 중인 로봇은 2 x 1 크기의 로봇으로 무지는 0과 1로 이루어진 N x N 크기의 지도에서 2 x 1 크기인 로봇을 움직여 (N, N) 위치까지 이동 할 수 있도록 프로그래밍을 하려고 합니다. 로봇이 이동하는 지도는 가장 왼쪽, 상단의 좌표를 (1, 1)로 하며 지도 내에 표시된 숫자 0은 빈칸을 1은 벽을 나타냅니다. 로봇은 벽이 있는 칸 또는 지도 밖으로는 이동할 수 없습니다. 로봇은 처음에 아래 그림과 같이 좌표 (1, 1) 위치에서 가로방향으로 놓여있는 상태로 시작하며, 앞뒤 구분없이 움직일 수 있습니다.

블럭이동-1.jpg

로봇이 움직일 때는 현재 놓여있는 상태를 유지하면서 이동합니다. 예를 들어, 위 그림에서 오른쪽으로 한 칸 이동한다면 (1, 2), (1, 3) 두 칸을 차지하게 되며, 아래로 이동한다면 (2, 1), (2, 2) 두 칸을 차지하게 됩니다. 로봇이 차지하는 두 칸 중 어느 한 칸이라도 (N, N) 위치에 도착하면 됩니다.

로봇은 다음과 같이 조건에 따라 회전이 가능합니다.

블럭이동-2.jpg

위 그림과 같이 로봇은 90도씩 회전할 수 있습니다. 단, 로봇이 차지하는 두 칸 중, 어느 칸이든 축이 될 수 있지만, 회전하는 방향(축이 되는 칸으로부터 대각선 방향에 있는 칸)에는 벽이 없어야 합니다. 로봇이 한 칸 이동하거나 90도 회전하는 데는 걸리는 시간은 정확히 1초 입니다.

0과 1로 이루어진 지도인 board가 주어질 때, 로봇이 (N, N) 위치까지 이동하는데 필요한 최소 시간을 return 하도록 solution 함수를 완성해주세요.

### 제한사항
---
board의 한 변의 길이는 5 이상 100 이하입니다.
board의 원소는 0 또는 1입니다.
로봇이 처음에 놓여 있는 칸 (1, 1), (1, 2)는 항상 0으로 주어집니다.
로봇이 항상 목적지에 도착할 수 있는 경우만 입력으로 주어집니다.

### 입출력 예
---
board	result
[[0, 0, 0, 1, 1],[0, 0, 0, 1, 0],[0, 1, 0, 1, 1],[1, 1, 0, 0, 1],[0, 0, 0, 0, 0]]	7

#### 입출력 예에 대한 설명
문제에 주어진 예시와 같습니다.
로봇이 오른쪽으로 한 칸 이동 후, (1, 3) 칸을 축으로 반시계 방향으로 90도 회전합니다. 다시, 아래쪽으로 3칸 이동하면 로봇은 (4, 3), (5, 3) 두 칸을 차지하게 됩니다. 이제 (5, 3)을 축으로 시계 방향으로 90도 회전 후, 오른쪽으로 한 칸 이동하면 (N, N)에 도착합니다. 따라서 목적지에 도달하기까지 최소 7초가 걸립니다.

<br>

### 풀이
---

1. 기본 BFS 문제 + 움직이는 놈이 2칸을 차지 + 회전이 가능
2. 움직이는 놈이 2칸을 차지하는데 각각의 y좌표와 x좌표를 저장하지 말고 짝이 되는 블록이 어느 방향에 있는지 저장해준다.
    - point 구조체 참고

3. 이동의 경우는 상하좌우 이동이 가능하다
4. 회전의 경우 가로 방향에서 회전하는 경우와 세로 방향에서 회전하는 경우가 있다.
    - 두 경우 모두 회전하는 방향과 맞닿은 면에 1이 하나라도 존재하면 회전을 하지 못한다는 점을 주의하자



```c++

#include <vector>
#include <algorithm>
#include <iostream>
#include <queue>
using namespace std;

struct point {
	int y;
	int x;
	int dir;
};

pair<int, int> dir[] = { {0, 1}, {1, 0}, {0, -1}, {-1, 0} };
int rotateDir[] = { 1, -1 };

int solution(vector<vector<int>> board) {

	int answer = 0;

	point p;
	p.y = 0; p.x = 0; p.dir = 0;

	queue<pair<point, int>> que;
	int N = board.size();
	bool visited[100][100][4] = { false, };

	que.push({ p, 0 });

	point curP, curP2, nextP, nextP2;
	int cnt;
	while (!que.empty()) {

		curP = que.front().first;
		curP2.y = curP.y + dir[curP.dir].first;
		curP2.x = curP.x + dir[curP.dir].second;
		cnt = que.front().second;
		que.pop();

		if (curP.y == N - 1 && curP.x == N - 1) {
			answer = cnt;
			break;
		}
		if (curP2.y == N - 1 && curP2.x == N - 1) {
			answer = cnt;
			break;
		}

		if (curP.y < 0 || curP.y >= N || curP.x < 0 || curP.x >= N
			|| curP2.y < 0 || curP2.y >= N || curP2.x < 0 || curP2.x >= N) continue;

		if (visited[curP.y][curP.x][curP.dir]) continue;

		visited[curP.y][curP.x][curP.dir] = true;

		// 이동
		nextP.dir = curP.dir;
		for (int i = 0; i < 4; i++) {
			nextP.y = curP.y + dir[i].first;
			nextP.x = curP.x + dir[i].second;
			nextP2.y = curP2.y + dir[i].first;
			nextP2.x = curP2.x + dir[i].second;

			if (nextP.y >= 0 && nextP.y < N && nextP.x >= 0 && nextP.x < N
				&& nextP2.y >= 0 && nextP2.y < N && nextP2.x >= 0 && nextP2.x < N
				&& !visited[nextP.y][nextP.x][nextP.dir]
				&& board[nextP.y][nextP.x] == 0 && board[nextP2.y][nextP2.x] == 0) {
				que.push(make_pair(nextP, cnt+1));
			}

		}

		// 세로 방향
		if (curP.dir == 1 || curP.dir == 3) {

			for (int i = 0; i < 2; i++) {

				nextP.x = curP.x + rotateDir[i];
				nextP2.x = curP2.x + rotateDir[i];

				if (nextP.x >= 0 && nextP.x < N && nextP2.x >= 0 && nextP2.x < N
					&& board[curP.y][nextP.x] == 0 && board[curP2.y][nextP2.x] == 0){

					if(!visited[curP.y][curP.x][2 * i]){
						nextP.y = curP.y;
						nextP.x = curP.x;
						nextP.dir = 2 * i;
						que.push(make_pair(nextP, cnt + 1));
					}

					if (!visited[curP2.y][curP2.x][2 * i]) {
						nextP.y = curP2.y;
						nextP.x = curP2.x;
						nextP.dir = 2 * i;
						que.push(make_pair(nextP, cnt + 1));
					}

				}
			}

		}

		// 가로 방향
		else {
			for (int i = 0; i < 2; i++) {
				nextP.y = curP.y + rotateDir[i];
				nextP2.y = curP2.y + rotateDir[i];

				if (nextP.y >= 0 && nextP.y < N && nextP2.y >= 0 && nextP2.y < N
					&& board[nextP.y][curP.x] == 0 && board[nextP2.y][curP2.x] == 0){

					if (!visited[curP.y][curP.x][2 * i + 1]) {
						nextP.y = curP.y;
						nextP.x = curP.x;
						nextP.dir = 2 * i + 1;
						que.push(make_pair(nextP, cnt + 1));
					}

					if (!visited[curP2.y][curP2.x][2 * i + 1]) {
						nextP.y = curP2.y;
						nextP.x = curP2.x;
						nextP.dir = 2 * i + 1;
						que.push(make_pair(nextP, cnt + 1));
					}

				}

			}
		}

	}


	return answer;
}

```

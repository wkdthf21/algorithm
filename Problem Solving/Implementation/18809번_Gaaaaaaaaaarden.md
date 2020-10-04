18809번 : Gaaaaaaaaaarden
===
### 문제
---
길고 길었던 겨울이 끝나고 BOJ 마을에도 봄이 찾아왔다. BOJ 마을에서는 꽃을 마을 소유의 정원에 피우려고 한다. 정원은 땅과 호수로 이루어져 있고 2차원 격자판 모양이다.

인건비 절감을 위해 BOJ 마을에서는 직접 사람이 씨앗을 심는 대신 초록색 배양액과 빨간색 배양액을 땅에 적절하게 뿌려서 꽃을 피울 것이다. 이 때 배양액을 뿌릴 수 있는 땅은 미리 정해져있다.

배양액은 매 초마다 이전에 배양액이 도달한 적이 없는 인접한 땅으로 퍼져간다.

아래는 초록색 배양액 2개를 뿌렸을 때의 예시이다. 하얀색 칸은 배양액을 뿌릴 수 없는 땅을, 황토색 칸은 배양액을 뿌릴 수 있는 땅을, 하늘색 칸은 호수를 의미한다.



초록색 배양액과 빨간색 배양액이 동일한 시간에 도달한 땅에서는 두 배양액이 합쳐져서 꽃이 피어난다. 꽃이 피어난 땅에서는 배양액이 사라지기 때문에 더 이상 인접한 땅으로 배양액을 퍼트리지 않는다.

아래는 초록색 배양액 2개와 빨간색 배양액 2개를 뿌렸을 때의 예시이다.



배양액은 봄이 지나면 사용할 수 없게 되므로 주어진 모든 배양액을 남김없이 사용해야 한다. 예를 들어 초록색 배양액 2개와 빨간색 배양액 2개가 주어졌는데 초록색 배양액 1개를 땅에 뿌리지 않고, 초록색 배양액 1개와 빨간색 배양액 2개만을 사용하는 것은 불가능하다.

또한 모든 배양액은 서로 다른 곳에 뿌려져야 한다.

정원과 두 배양액의 개수가 주어져있을 때 피울 수 있는 꽃의 최대 개수를 구해보자.

<br>

### 입력
---
첫째 줄에 정원의 행의 개수와 열의 개수를 나타내는 N(2 ≤ N ≤ 50)과 M(2 ≤ M ≤ 50), 그리고 초록색 배양액의 개수 G(1 ≤ G ≤ 5)와 빨간색 배양액의 개수 R(1 ≤ R ≤ 5)이 한 칸의 빈칸을 사이에 두고 주어진다.

그 다음 N개의 줄에는 각 줄마다 정원의 각 행을 나타내는 M개의 정수가 한 개의 빈 칸을 사이에 두고 주어진다. 각 칸에 들어가는 값은 0, 1, 2이다. 0은 호수, 1은 배양액을 뿌릴 수 없는 땅, 2는 배양액을 뿌릴 수 있는 땅을 의미한다.

배양액을 뿌릴 수 있는 땅의 수는 R+G개 이상이고 10개 이하이다.

<br>

### 출력
---
첫째 줄에 피울 수 있는 꽃의 최대 개수를 출력한다.

<br>


### 풀이
---

- green과 red가 퍼진 시간을 저장하는 map을 각각 만든다.
- green과 red 각각 bfs를 돌릴 queue를 만들고 green부터 퍼뜨린다.
- 이때 red가 퍼질 때 꽃이 만들어지는데, 꽃이 된 좌표는 green queue에 들어있으므로 flower 맵을 사용해서 flower가 된 좌표인지 체크해준다.
  - flower는 퍼뜨릴 수 없다

- position vector에 있는 좌표들로 순열을 생성해서 돌리면 중복되는 경우가 많아진다.
  - G = 2이고 R = 1일 때
  - position : { {0, 1}, {1, 0}, {1, 2}, {3, 4} } 와
  - position : { {1, 0}, {0, 1}, {1, 2}, {3, 4} } 은 사실 같은 경우이다

- 위에 대로 하면 시간초과
- 따라서 loc 배열을 따로 두고 아무것도 아닌 경우를 0, G는 1, R은 2로 설정한뒤 정렬하고 순열을 생성해서 위 문제를 해결하였다.

<br>

```c++
#include <string>
#include <vector>
#include <iostream>
#include <algorithm>
#include <map>
#include <queue>
#include <memory.h>
using namespace std;

const int MAX = 50 + 1;
vector<pair<int, int>> position;
vector<int> loc;
int N, M, G, R;
int m[MAX][MAX];
int dy[] = { -1, 1, 0, 0 };
int dx[] = { 0, 0, -1 ,1 };

queue<pair<int, int>> gque;
queue<pair<int, int>> rque;
int gtime[MAX][MAX];
int rtime[MAX][MAX];
int flower[MAX][MAX];

int bfs() {

	int ans = 0;
	int ctime = 0;
	while (!gque.empty() || !rque.empty()) {

		int gsize = gque.size();
		int rsize = rque.size();
		++ctime;

		while (gsize--) {

			pair<int, int> p = gque.front();
			gque.pop();

			if (flower[p.first][p.second] != -1) continue; // 꽃은 전파 불가능

			for (int i = 0; i < 4; i++) {
				int ny = p.first + dy[i];
				int nx = p.second + dx[i];
				if (ny < 0 || nx < 0 || ny >= N || nx >= M) continue;
				if (gtime[ny][nx] != -1) continue;
				if (rtime[ny][nx] != -1) continue;
				if (m[ny][nx] == 0) continue;
				gtime[ny][nx] = ctime;
				gque.push(make_pair(ny, nx));
			}

		}

		while (rsize--) {

			pair<int, int> p = rque.front();
			rque.pop();

			if (flower[p.first][p.second] != -1) continue; // 꽃은 전파 불가능

			for (int i = 0; i < 4; i++) {
				int ny = p.first + dy[i];
				int nx = p.second + dx[i];
				if (ny < 0 || nx < 0 || ny >= N || nx >= M) continue;
				if (rtime[ny][nx] != -1) continue;
				if (m[ny][nx] == 0) continue;
				if (ctime != 0 && gtime[ny][nx] == ctime) {
					rtime[ny][nx] = ctime;
					flower[ny][nx] = ctime;
					++ans;				}
				if (gtime[ny][nx] == -1) {
					rtime[ny][nx] = ctime;
					rque.push(make_pair(ny, nx));
				}
			}
		}

	}


	return ans;

}


int main() {

	ios::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);


	cin >> N >> M >> G >> R;
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < M; j++) {
			cin >> m[i][j];
			if (m[i][j] == 2) {
				position.push_back(make_pair(i, j));
				loc.push_back(0);
			}
		}
	}

	for (int i = 0; i < loc.size(); i++) {
		if (G > 0) {
			loc[i] = 1;
			--G;
		}
		else if (R > 0) {
			loc[i] = 2;
			--R;
		}
	}

	sort(loc.begin(), loc.end());

	int ans = 0;
	do {

		memset(gtime, -1, sizeof(gtime));
		memset(rtime, -1, sizeof(rtime));
		memset(flower, -1, sizeof(flower));

		for (int i = 0; i < position.size(); i++) {
			if (loc[i] == 0) continue;
			else if (loc[i] == 1) {
				gtime[position[i].first][position[i].second] = 0;
				gque.push({ position[i].first, position[i].second });
			}
			else if (loc[i] == 2) {
				rtime[position[i].first][position[i].second] = 0;
				rque.push({ position[i].first, position[i].second });
			}
		}

		ans = max(ans, bfs());

	} while (next_permutation(loc.begin(), loc.end()));

	cout << ans << endl;

	return 0;

}

```

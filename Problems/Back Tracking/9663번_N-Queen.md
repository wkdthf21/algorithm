9663번 : N-Queen
=============

### 문제
***
N-Queen 문제는 크기가 N × N인 체스판 위에 퀸 N개를 서로 공격할 수 없게 놓는 문제이다.

N이 주어졌을 때, 퀸을 놓는 방법의 수를 구하는 프로그램을 작성하시오.

<br>

### 입력
***

첫째 줄에 N이 주어진다. (1 ≤ N < 15)

<br>

### 출력
***

첫째 줄에 퀸 N개를 서로 공격할 수 없게 놓는 경우의 수를 출력한다.

<br>

### 풀이
***

퀸이 위치하면 전체 가로/세로/대각선의 위치에는 퀸이 놓일 수 없다. <br>
DFS를 이용해 모든 경우를 탐색하였는데, <br>
자식 노드가 유망한지 검사는 새로 퀸을 놓을 떄 마다 <br>
1)일렬에 퀸이 위치했는지<br>
2)대각선에 퀸이 위치했는지<br>
arr 배열을 이용해 검사를 한다.<br>
첫번째 시도에 퀸을 2번 위치에 놓는 경우 arr[1] = 2 이다.


```c++
#include <iostream>
#include <cmath>

using namespace std;

const int MAX = 15 + 1;
int N = 0;
int answer = 0;
int arr[MAX] = { 0 };

bool IsPromising(int idx) {
	for (int i = 1; i < idx; i++) {
		if (arr[i] == arr[idx]) // 일렬
			return false;
		if (abs(arr[i] - arr[idx]) == abs(i - idx)) // 대각선
			return false;
	}
	return true;
}

void N_Queens(int count) {

	if (count == N) {
		++answer;
		return;
	}

	for (int i = 1; i <= N; i++) {
		arr[count+1] = i;
		if(IsPromising(count+1))
			N_Queens(count + 1);
	}

	return;
}

int main() {

	cin >> N;

	N_Queens(0);

	cout << answer << "\n";

	return 0;
}
```

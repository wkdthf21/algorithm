10090번 : Counting Inversions
===

### 문제
---
A permutation of integers from 1 to n is a sequence a1, a2, ..., an, such that each integer from 1 to n is appeared in the sequence exactly once.

Two integers in а permutation form an inversion, when the bigger one is before the smaller one.

As an example, in the permutation 4 2 7 1 5 6 3, there are 10 inversions in total. They are the following pairs: 4–2, 4–1, 4–3, 2–1, 7–1, 7–5, 7–6, 7–3, 5–3, 6–3.

Write program invcnt that computes the number of the inversions in a given permutation.

<br>

### 입력
---
The value for the number n is written on the first line of the standard input. The permutation is written on the second line: n numbers, delimited by spaces. (2 ≤ n ≤ 1000000)

<br>

### 출력
---
Write the count of inversions on the standard output.

<br>

### 풀이
---

- Counting Inversion이란?
  - 순열이 주어지고 i < j 일때, Pi > Pj 인 수가 몇 쌍이나 있는지 세는 문제이다.
- Brute Force로 풀면 무려 O(N^2) 이라는 시간이 나온다.
- 이 문제는 Merge Sort 방식에서 아이디어를 따와서 생각해 볼 수 있다!
- 숫자 1 3 4 / 2 5 와 같이 Merge Sort에서 Merge 중에 정렬된 두 개의 배열이 있다고 해보자
  - 그럼 1 과 2를 비교할 것이다.
  - 1이 작으므로 1부터 임시 배열에 들어갈 것이다.
  - 그 다음 3과 2를 비교한다.
  - 2가 더 작다. 2는 왼쪽 배열에서 3, 4 즉 2개보다 작다. 2개의 inversion 발생!
  - 임시 배열에는 1 2 가 들어가고
  - 나머지 요소들을 위에서와 마찬가지로 진행한다.


<br>

```c++
#include<iostream>

using namespace std;

int N, num[1000001], temp[1000001];
long long answer = 0;

void merge(int left, int right) {

	int mid = (left + right) / 2;
	int i = left;
	int j = mid + 1;
	int k = left;

	while (i <= mid && j <= right) {
		if (num[i] < num[j]) {
			temp[k++] = num[i++];
		}
		else {
			answer += mid - i + 1;
			temp[k++] = num[j++];
		}
	}

	int s = (i <= mid ? i : j);

	while (k <= right)
		temp[k++] = num[s++];

	for (int l = left; l <= right; l++)
		num[l] = temp[l];

}


void partition(int left, int right) {

	if (left < right) {
		int inversionCnt = 0;
		int mid = (left + right) / 2;
		partition(left, mid);
		partition(mid + 1, right);
		merge(left, right);
	}

}

int main(int argc, char** argv)
{
		cin >> N;
		for (int i = 0; i < N; i++)
			cin >> num[i];

		partition(0, N - 1);
		cout << answer << "\n";

	return 0;
}
```

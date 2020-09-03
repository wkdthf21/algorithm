2020 KAKAO BLIND RECRUITMENT : 괄호 변환
===


### 문제 링크
---

https://programmers.co.kr/learn/courses/30/lessons/60058

<br>

### 문제 풀이
---

```c++
#include <vector>
#include <algorithm>
#include <string>
#include <stack>
using namespace std;

// 첫번째와 마지막 문자를 제거하고 괄호방향을 뒤집음
string convertString(string s) {
	string temp = "";
	string result = "";

	if (s.size() > 2)
		temp = s.substr(1, s.size() - 2);

	for (int i = 0; i < temp.size(); i++) {
		if (temp[i] == '(') result += ")";
		else result += "(";
	}
	return result;
}


// 올바른 문자열 판별
bool isRightString(string s) {
	stack<char> st;
	for (int i = 0; i < s.size(); i++) {
		if (s[i] == '(') st.push('(');
		if (s[i] == ')') {
			if (st.empty()) return false;
			st.pop();
		}
	}
	if (st.empty()) return true;
	return false;
}

string solution(string p) {
	string answer = "";

	// step1
	if (p.compare("") == 0) return "";

	// step2
	int left = 0, right = 0, split = 0;
	for (int i = 0; i < p.size(); i++) {
		if (p[i] == '(') {
			++left;
		}
		if (p[i] == ')') {
			++right;
		}
		if (left == right) {
			split = i + 1;
			break;
		}
	}

	string u = p.substr(0, split);
	string v = p.substr(split, p.size() - split);

	// step3
	if (isRightString(u)) {
		answer += u + solution(v);
	}

	// step4
	else {
		string temp = "(" + solution(v) + ")";
		temp += convertString(u);
		answer += temp;
	}


	return answer;
}
```

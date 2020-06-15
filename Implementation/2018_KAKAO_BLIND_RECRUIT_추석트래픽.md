2018 KAKAO BLIND RECRUIT : 추석트래픽
====================
### 문제 설명
---
이번 추석에도 시스템 장애가 없는 명절을 보내고 싶은 어피치는 서버를 증설해야 할지 고민이다. 장애 대비용 서버 증설 여부를 결정하기 위해 작년 추석 기간인 9월 15일 로그 데이터를 분석한 후 초당 최대 처리량을 계산해보기로 했다. 초당 최대 처리량은 요청의 응답 완료 여부에 관계없이 임의 시간부터 1초(=1,000밀리초)간 처리하는 요청의 최대 개수를 의미한다.

<br>

### 입력 형식
---
solution 함수에 전달되는 lines 배열은 N(1 ≦ N ≦ 2,000)개의 로그 문자열로 되어 있으며, 각 로그 문자열마다 요청에 대한 응답완료시간 S와 처리시간 T가 공백으로 구분되어 있다.
응답완료시간 S는 작년 추석인 2016년 9월 15일만 포함하여 고정 길이 2016-09-15 hh:mm:ss.sss 형식으로 되어 있다.
처리시간 T는 0.1s, 0.312s, 2s 와 같이 최대 소수점 셋째 자리까지 기록하며 뒤에는 초 단위를 의미하는 s로 끝난다.
예를 들어, 로그 문자열 2016-09-15 03:10:33.020 0.011s은 2016년 9월 15일 오전 3시 10분 **33.010초**부터 2016년 9월 15일 오전 3시 10분 **33.020초**까지 **0.011초** 동안 처리된 요청을 의미한다. (처리시간은 시작시간과 끝시간을 포함)
서버에는 타임아웃이 3초로 적용되어 있기 때문에 처리시간은 0.001 ≦ T ≦ 3.000이다.
lines 배열은 응답완료시간 S를 기준으로 오름차순 정렬되어 있다.

<br>

### 출력 형식
---
solution 함수에서는 로그 데이터 lines 배열에 대해 초당 최대 처리량을 리턴한다.

<br>

### 풀이
---

1. 응답 처리 시작과 끝에서만 현재 처리하고 있는 일의 개수가 달라진다
2. 따라서 응답 처리 시작에서 1초를 체크하거나 응답 처리 끝에서 1초를 체크하면 된다
3. 나는 응답 처리 시간의 끝에서부터 1초 동안, 처리되고 있는 응답의 개수를 카운팅했다.
  - 주의할 부분 : **부동소수점의 오차범위로 인해 같은 값의 double값 비교 시 == 가 false를 리턴할 수 있다.**

<br>

```c++
#include <string>
#include <vector>

#define abs(x) (x) < 0 ? (-x) : (x)
#define EPSILON 0.001
using namespace std;

vector<string> strtokenize(string str, char delim){

    vector<string> result;
    int prev = 0;
    for(int i = 0; i < str.size(); i++){
        if(str[i] == delim){
            result.push_back(str.substr(prev, i-prev));
            prev = i+1;
        }
    }

    // 마지막 덩어리
    if(prev != str.size())
        result.push_back(str.substr(prev, str.size() - prev));

    return result;
}


int solution(vector<string> lines) {
    int answer = 0;

    // lines 문자열 파싱 -> double {시작, 끝 저장}
    vector<pair<double, double>> timeVec;
    for(int i = 0; i < lines.size(); i++){
        string line = lines[i];

        vector<string> log = strtokenize(line, ' ');
        vector<string> time = strtokenize(log[1], ':');

        double performanceTime = stod(strtokenize(log[2], 's')[0]);
        double end = stod(time[0]) * 3600 + stod(time[1]) * 60 + stod(time[2]);
        timeVec.push_back(make_pair(end - performanceTime + 0.001, end));
    }

    // 응답 완료 시간을 기준으로
    for(int i = 0; i < timeVec.size(); i++){
        int cnt = 1;
        double begin = timeVec[i].second;
        double end = begin + 0.999;

        for(int j = i + 1; j < timeVec.size(); j++){
            if((abs(timeVec[j].first - end) <= EPSILON || timeVec[j].first < end) &&
               (abs(timeVec[j].second - begin) <= EPSILON || timeVec[j].second > begin))
                ++cnt;
        }
        answer = max(answer, cnt);

    }

    return answer;
}
```

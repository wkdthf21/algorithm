### 문제설명
***
라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.

방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.
네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.
각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.
음악이 00:00를 넘겨서까지 재생되는 일은 없다.
조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.
조건이 일치하는 음악이 없을 때에는 `(None)`을 반환한다.

### 입력형식
***
입력으로 네오가 기억한 멜로디를 담은 문자열 m과 방송된 곡의 정보를 담고 있는 배열 musicinfos가 주어진다.

m은 음 1개 이상 1439개 이하로 구성되어 있다.
musicinfos는 100개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 ','로 구분된 문자열이다.
음악의 시작 시각과 끝난 시각은 24시간 HH:MM 형식이다.
음악 제목은 ',' 이외의 출력 가능한 문자로 표현된 길이 1 이상 64 이하의 문자열이다.
악보 정보는 음 1개 이상 1439개 이하로 구성되어 있다.

### 출력형식
***
조건과 일치하는 음악 제목을 출력한다.

### 풀이
***
\#이 붙은 문자를 어떻게 처리할지 생각해보면 되는 문제  
들어온 문자열을 토큰화해서 비교하거나, 치환하는 방법을 사용  
--> C# -> c 로 치환하는 등 사용되지 않은 문자를 이용  

```c++
#include <string>
#include <vector>
#include <map>
#include <iostream>

using namespace std;


map<string, string> noteMap = {  {"C#", "c"}, {"D#", "d"}, {"F#", "f"}, {"G#", "g"}, {"A#", "a"}   };

vector<string> splitString(string inputStr, char delimiter) {

    vector<string> tokens;
    int start = 0;
    int end = inputStr.find(delimiter);
    int len = end - start;
    while(end != string::npos){
        string tmpStr = inputStr.substr(start, len);
        tokens.push_back(tmpStr);
        start = end + 1;
        end = inputStr.find(delimiter, start);
        len = end - start;
    }

    tokens.push_back(inputStr.substr(start, inputStr.size()-start));

    return tokens;
}


// timeStr1 < timeStr2
int getTimeDiffer(string timeStr1, string timeStr2){     

    int hour1 = atoi(timeStr1.substr(0, 2).c_str());
    int hour2 = atoi(timeStr2.substr(0, 2).c_str());
    int min1 = atoi(timeStr1.substr(3, 2).c_str());
    int min2 = atoi(timeStr2.substr(3, 2).c_str());

    if(hour1 < hour2)
        return hour2 * 60 + min2 - hour1 * 60 - min1;

    else if(hour1 == hour2)
        return min2 - min1;

    return 0;

}


string solution(string m, vector<string> musicinfos) {

    string answer = "(None)";
    int maxMin = -999;
    vector<string> answerVec;

    for(auto iter = musicinfos.begin(); iter != musicinfos.end(); ++iter){
        string musicInfo = *iter;
        char delimeter = ',';
        vector<string> musicvec = splitString(musicInfo, delimeter);
        string title = musicvec[2];
        int minDiffer = getTimeDiffer(musicvec[0], musicvec[1]);
        string noteInfo = musicvec[3];
        int noteInfoSize = noteInfo.size();

        // note 정보 토큰화 및 string 변환 + 치환
        vector<string> noteTokens;
        int limit = minDiffer;
        string noteString = "";
        for(int i = 0; i < limit; i++){
            string note = noteInfo.substr(i % noteInfoSize, 1);
            if(i == limit - 1){
                string nextNote = noteInfo.substr((i+1) % noteInfoSize, 1);
                if(note != "#" && nextNote == "#"){
                    string subtition = noteMap[note + "#"];
                    noteString += subtition;
                    break;
                }
            }
            if(note == "#"){
                string prev = noteInfo.substr((i-1) % noteInfoSize, 1);
                noteString = noteString.substr(0, noteString.size()-1);
                noteString += noteMap[prev + "#"];
                ++limit;
            }
            else{
                noteString += note;
            }
        }

        // m 토큰화 및 string 변환 + 치환
        string mString = "";
        int mSize = m.size();
        for(int i = 0; i < mSize; i++){
            string note = m.substr(i, 1);
            if(note == "#"){
                string prev = m.substr(i-1, 1);
                mString = mString.substr(0, mString.size()-1);
                mString += noteMap[prev + "#"];
            }
            else
                mString += note;
        }

        // get answer
        if(noteString.find(mString) != string :: npos){
            if(maxMin < minDiffer){
                maxMin = minDiffer;
                answerVec.clear();
                answerVec.push_back(title);
            }
            else if(maxMin == minDiffer)
                answerVec.push_back(title);
        }
    }

    if(!answerVec.empty())
        return answerVec.front();

    return answer;
}
```

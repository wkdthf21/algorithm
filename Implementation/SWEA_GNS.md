```c++
#include <iostream>
#include <map>
using namespace std;

int main(int argc, char** argv)
{
	int test_case;
	int T;

	cin>>T;

  string test_case_num;
  int size;
  string numberList[10] = {"ZRO", "ONE", "TWO", "THR", "FOR", "FIV", "SIX", "SVN", "EGT", "NIN"};

  for(test_case = 1; test_case <= T; ++test_case)
	{
        	cin >> test_case_num >> size;
        	map<string, int> numberMap = {{"ZRO", 0}, {"ONE", 0}, {"TWO", 0}, {"THR", 0}, {"FOR", 0}, {"FIV", 0}, {"SIX", 0}, {"SVN", 0}, {"EGT", 0}, {"NIN", 0}};
        	for(int i = 0; i < size; i++){
                string num;
                cin >> num;
                numberMap[num] = numberMap[num] + 1;
            }

        	cout << test_case_num << endl;
        	for(int i = 0; i < 10; i++){
                string key = numberList[i];
                int value = numberMap[key];
                for(int i = 0; i < value; i++){
                    cout << key << " ";
                }
            }
	}

	return 0;
}
```

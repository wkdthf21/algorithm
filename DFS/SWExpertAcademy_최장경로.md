```
#include<iostream>
#include <vector>
#include <memory.h>

using namespace std;

#define MAX 11
bool visited[MAX];
int dist = 0;

void dfs(int start, int count, vector<int> edge[MAX]){

    visited[start] = true;

    if(count > dist){
    	dist = count;
    }

    for(int i = 0; i < edge[start].size(); i++){
    	int next = edge[start][i];
        if(!visited[next]){
        	dfs(next, count+1, edge);
            visited[next] = false;
        }
    }

}

int main(int argc, char** argv)
{
	int test_case;
	int T;

	cin>>T;

	for(test_case = 1; test_case <= T; ++test_case)
	{
		int N, M;
        cin >> N >> M;

		vector<int> edge[MAX];

        int x, y;
        for(int i = 0; i < M; i++){
        	cin >> x >> y;
            edge[x].push_back(y);
            edge[y].push_back(x); // 무방향 그래프
        }


        // 시작 노드를 바꿔가며 최장 길이를 찾는다
        int maxDist = 0;
        for(int i = 1; i <= N; i++){
            memset(visited, false, sizeof(visited));
            dist = 0;
        	dfs(i, 1, edge);
            if(maxDist < dist){
            	maxDist = dist;
            }
        }

        cout << "#" << test_case << " " << maxDist << endl;

	}
  
	return 0;
}
```

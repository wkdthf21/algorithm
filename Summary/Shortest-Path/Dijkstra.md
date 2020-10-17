다익스트라(Dijkstra) 알고리즘
===

### 다익스트라 알고리즘?
---
하나의 정점에서 다른 모든 정점까지의 최단 경로를 탐색하는 알고리즘으로 DP를 활용
<br>

### 다익스트라 알고리즘이 DP 문제인 이유?
---

- 최단 거리는 여러 개의 최단 거리로 이루어져 있다.
- 최단 거리를 구할 때 이전에 구한 최단 거리 정보를 이용해서 최단 거리를 갱신한다.

<br>


### 특징
***

- 음의 간선을 포함하지 않기 때문에 다익스트라는 현실 세계에 적합하다.
- 인접 행렬과 최소 거리 노드 선형 탐색을 이용해 구현하면 시간 복잡도는 O(N^2)
- 최소 거리 노드 탐색 시 최소힙을 이용하면 시간 복잡도는 O(NlogN)


### 동작
***

1. 출발 노드를 설정한다.
2. 출발 노드를 기준으로 각 노드의 최소 비용을 저장한다.
3. 방문하지 않은 노드 중 cost가 가장 작은 노드를 선택한다.
4. 출발 노드에서 해당 노드를 거쳐 다른 노드로 가는 비용을 모든 노드에 대해 계산하고, 각각 기존에 구한 최단 거리 정보보다 작다면 갱신한다.
5. 3 ~ 4를 반복한다. (모든 노드들에 방문할 때 까지)


<br>

### 코드
***

1. 최소 힙

```java
import java.util.*

class Solution {

    final int INF = 1000000;
    int dist[] = new int [5];    // 최단 거리 저장
    boolean[] isSelected = new boolean[5];

    public static void main(String[] args) {

        int m[][] = {{INF, 1, INF, 2, INF},
                     {1, INF, 3, INF, 2},
                     {INF, 3, INF, INF, 1},
                     {2, INF, INF, INF, 2},
                     {INF, 2, 1, 2, INF}};

      doDijkstra(5, m);

    }


    public void doDijkstra(int N, int[][] m) {

        // 초기화
        Arrays.fill(dist, INF);
        Arrays.fill(isSelected, false);

        PriorityQueue<Integer> pq = new PriorityQueue<>(new Comparator<Integer>(){
            @Override
            public int compare(Integer n1, Integer n2){
                return Integer.compare(dist[n1], dist[n2]);
            }
        });

        // 시작 노드
        dist[0] = 0;
        pq.add(0);

        while(!pq.isEmpty()){
            int cur = pq.poll();
            if(isSelected[cur]) continue;
            isSelected[cur] = true;

            for(int i = 0; i < N; i++){
                if(m[cur][i] == INF) continue;
                int temp = dist[cur] + m[cur][i];
                if(temp < dist[i]){
                    dist[i] = temp;
                    pq.offer(i); // 최소 힙에 갱신한 모든 값을 넘겨준다
                }
            }
        }

    }
}


```


2. 선형 탐색

```java
import java.util.*

class Solution {

    final int INF = 1000000;
    int dist[] = new int [5];    // 최단 거리 저장
    boolean[] isSelected = new boolean[5];

    public static void main(String[] args) {

        int m[][] = {{INF, 1, INF, 2, INF},
                     {1, INF, 3, INF, 2},
                     {INF, 3, INF, INF, 1},
                     {2, INF, INF, INF, 2},
                     {INF, 2, 1, 2, INF}};

      doDijkstra(5, m);

    }


    public void doDijkstra(int N, int[][] m) {

        // 초기화
        Arrays.fill(dist, INF);
        Arrays.fill(isSelected, false);

        PriorityQueue<Integer> pq = new PriorityQueue<>(new Comparator<Integer>(){
            @Override
            public int compare(Integer n1, Integer n2){
                return Integer.compare(dist[n1], dist[n2]);
            }
        });

        // 시작 노드
        for(int i = 0; i < N; i++) dist[i] = m[0][i];
        dist[0] = 0;
        isSelected[0] = true;

        // 다익스트라
        for(int i = 0; i < N-1; i++){

          // 선택되지 않은 최소 노드 찾기
          int min = INF;
          int minIdx = 0;
          for(int j = 0; j < N; j++){
              if(isSelected[j]) continue;
              if(dist[j] < min){
                min = dist[j];
                minIdx = j;
              }
          }

          isSelected[minIdx] = true;

          for(int j = 0; j < N; j++){

            if(m[minIdx][j] == INF) continue;
            if(dist[minIdx] + m[minIdx][j] < dist[j]){
              dist[j] = dist[minIdx] + m[minIdx][j];
            }

          }

        }

    }
}
```

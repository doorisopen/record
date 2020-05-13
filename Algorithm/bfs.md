# 너비 우선 탐색(BFS, Breadth First Search)
* 다차원 배열에서 각 칸을 방문할 때 __너비를 우선으로 방문하는 알고리즘__
* BFS는 방문한 노드들을 차례로 저장하고 꺼낼 수 있는 __자료구조인 큐(Queue)를 사용__ 한다.
* __재귀적으로__ 동작하지 않는다.
* 'Prim', 'Dijkstra' 알고리즘과 유사하다.
* 출발노드에서 목표노드까지의 __최단 길이 경로를 보장__ 한다.
* 그래프 탐색의 경우 어떤 노드를 방문했었는지 여부를 반드시 검사 해야한다. 그렇지 않으면 __무한루프에__ 빠질 위험이 있다.(단점3)
* __단점__
  1. __경로가 매우 길 경우__ 에는 탐색 가지가 급격히 증가함에 따라 보다 많은 기억 공간을 필요로 하게 된다.
  2. __해가 존재하지 않는다면__ 유한 그래프(finite graph)의 경우에는 모든 그래프를 탐색한 후에 실패로 끝난다.
  3. __무한 그래프(infinite graph)의__ 경우에는 결코 해를 찾지도 못하고, 끝내지도 못한다.


![alogorithm_animated_bfs](/images/algorithm/alogorithm_animated_bfs.gif)


# BFS 예시 코드

```
#include <bits/stdc++.h>
using namespace std;
#define X first
#define Y second // pair에서 first, second를 줄여서 쓰기 위해서 사용
int board[502][502] =
{{1,1,1,0,1,0,0,0,0,0},
 {1,0,0,0,1,0,0,0,0,0},
 {1,1,1,0,1,0,0,0,0,0},
 {1,1,0,0,1,0,0,0,0,0},
 {0,1,0,0,0,0,0,0,0,0},
 {0,0,0,0,0,0,0,0,0,0},
 {0,0,0,0,0,0,0,0,0,0} }; // 1이 파란 칸, 0이 빨간 칸에 대응
bool vis[502][502]; // 해당 칸을 방문했는지 여부를 저장
int n = 7, m = 10; // n = 행의 수, m = 열의 수
int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1}; // 상하좌우 네 방향을 의미
int main(void){
  ios::sync_with_stdio(0);
  cin.tie(0);
  queue<pair<int,int> > Q;
  vis[0][0] = 1; // (0, 0)을 방문했다고 명시
  Q.push({0,0}); // 큐에 시작점인 (0, 0)을 삽입.
  while(!Q.empty()){
    pair<int,int> cur = Q.front(); Q.pop();
    cout << '(' << cur.X << ", " << cur.Y << ") -> ";
    for(int dir = 0; dir < 4; dir++){ // 상하좌우 칸을 살펴볼 것이다.
      int nx = cur.X + dx[dir];
      int ny = cur.Y + dy[dir]; // nx, ny에 dir에서 정한 방향의 인접한 칸의 좌표가 들어감
      if(nx < 0 || nx >= n || ny < 0 || ny >= m) continue; // 범위 밖일 경우 넘어감
      if(vis[nx][ny] || board[nx][ny] != 1) continue; // 이미 방문한 칸이거나 파란 칸이 아닐 경우
      vis[nx][ny] = 1; // (nx, ny)를 방문했다고 명시
      Q.push({nx,ny});
    }
  }
}
```
# BFS 문제 유형(+응용 유형)
* Flood Fill
* 거리 측정
* 시작점이 여러 개일 때
* 시작점이 두 종류일 때
* 1차원에서의 BFS

# 주의할점
* 시작점에 방문했다는 표시를 남기지 않는다.
* 큐에 넣을 때 방문했다는 표시를 하는 대신 큐에서 빼낼 때 방문했다는 표시를 남겼다.
* 이웃한 원소가 범위를 벗어났는지에 대한 체크를 잘못했다.

# BFS 연습문제
* [BOJ 1926: 그림](https://www.acmicpc.net/problem/1926)
* [BOJ 2178: 미로 탐색](https://www.acmicpc.net/problem/2178)
* [BOJ 7576: 토마토](https://www.acmicpc.net/problem/7576)
* [BOJ 4179: 불](https://www.acmicpc.net/problem/4179)
* [BOJ 1697: 숨바꼭질](https://www.acmicpc.net/problem/1697)

# Reference
* [[바킹독의 실전 알고리즘] 0x09강 - BFS](https://www.youtube.com/watch?v=ftOmGdm95XI)
* [commons.wikimedia.org](https://commons.wikimedia.org/wiki/File:Animated_BFS.gif)
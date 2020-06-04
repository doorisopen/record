---
title: 깊이 우선 탐색(DFS, Depth First Search)
category: Algorithm
comments: true
order: 5
---

# DFS(Depth First Search)
* 다차원 배열에서 각 칸을 방문할 때 __깊이__ 를 우선으로 방문하는 알고리즘
* DFS는 방문한 노드들을 차례로 저장하고 꺼낼 수 있는 __자료구조인 스택(Stack)를 사용__ 한다.
* DFS는 __그래프와 트리__ 자료구조에서 이용된다.
* DFS는 BFS와 같이 __다차원 배열의 거리 계산을 할 수 없다.__

### BFS(Breadth First Search)
* 다차원 배열에서 각 칸을 방문할 때 너비를 우선으로 방문하는 알고리즘

## DFS 동작 예시
1. 시작하는 칸을 스택에 넣고 방문했다는 표시를 남김
2. 스택에서 원소를 꺼내어 그 칸과 상하좌우로 인접한 칸에 대해 3번을 진행
3. 해당 칸을 이전에 방문했다면 아무 것도 하지 않고, 처음으로 방문했다면 방문했다는 표시를 남기고 해당 칸을 스택에 삽입
4. 스택이 빌 때 까지 2번을 반복

> 모든 칸이 스택에 1번씩 들어가므로 __시간복잡도는 칸이 N개일때 O(N)__


# DFS 예시 코드

```
#include <bits/stdc++.h>
using namespace std;
#define X first
#define Y second // pair에서 first, second를 줄여서 쓰기 위해서 사용
int board[502][502] =
{
 {1,1,1,0,1,0,0,0,0,0},
 {1,0,0,0,1,0,0,0,0,0},
 {1,1,1,0,1,0,0,0,0,0},
 {1,1,0,0,1,0,0,0,0,0},
 {0,1,0,0,0,0,0,0,0,0},
 {0,0,0,0,0,0,0,0,0,0},
 {0,0,0,0,0,0,0,0,0,0}
}; // 1이 파란 칸, 0이 빨간 칸에 대응
bool vis[502][502]; // 해당 칸을 방문했는지 여부를 저장
int n = 7, m = 10; // n = 행의 수, m = 열의 수
int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1}; // 상하좌우 네 방향을 의미
int main(void){
  ios::sync_with_stdio(0);
  cin.tie(0);
  stack<pair<int,int> > S;
  vis[0][0] = 1; // (0, 0)을 방문했다고 명시
  S.push({0,0}); // 스택에 시작점인 (0, 0)을 삽입.
  while(!S.empty()){
    pair<int,int> cur = S.top(); S.pop();
    cout << '(' << cur.X << ", " << cur.Y << ") -> ";
    for(int dir = 0; dir < 4; dir++){ // 상하좌우 칸을 살펴볼 것이다.
      int nx = cur.X + dx[dir];
      int ny = cur.Y + dy[dir]; // nx, ny에 dir에서 정한 방향의 인접한 칸의 좌표가 들어감
      if(nx < 0 || nx >= n || ny < 0 || ny >= m) continue; // 범위 밖일 경우 넘어감
      if(vis[nx][ny] || board[nx][ny] != 1) continue; // 이미 방문한 칸이거나 파란 칸이 아닐 경우
      vis[nx][ny] = 1; // (nx, ny)를 방문했다고 명시
      S.push({nx,ny});
    }
  }
}
```

# DFS와 BFS 방문순서 차이

![algorithm-dfs_bfs_move]({{ site.baseurl }}/images/Algorithm/algorithm-dfs_bfs_move.JPG)

# Reference
* [[바킹독의 실전 알고리즘] 0x0A강 - DFS](https://www.youtube.com/watch?v=93jy2yUYfVE)
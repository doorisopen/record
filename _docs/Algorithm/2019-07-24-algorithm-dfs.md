---
title: 깊이 우선 탐색(DFS, Depth First Search)
category: Algorithm
date: 2019-07-24 18:32:30
comments: true
order: 12
---

## DFS(Depth First Search)
* 다차원 배열에서 각 칸을 방문할 때 __깊이__ 를 우선으로 방문하는 알고리즘
* DFS는 방문한 노드들을 차례로 저장하고 꺼낼 수 있는 __자료구조인 스택(Stack)를 사용__ 한다.
* DFS는 __그래프와 트리__ 자료구조에서 이용된다.
* DFS는 BFS와 같이 __다차원 배열의 거리 계산을 할 수 없다.__
* DFS는 __'맹목적 탐색방법'__ 의 하나이다.<br>
  + __맹목적 탐색__ 이란, 이미 정해진 순서에 따라 상태 공간 그래프를 점차 형성해 가면서 해를 탐색하는 방법을 말한다.

## 깊이 우선 탐색(Depth First Search)의 특징
* __너비 우선 탐색(BFS)__ 에서는 __큐(queue)__ 가 사용되었지만 __깊이 우선 탐색(DFS)__ 에서는 __스택(stack)__ or __재귀적인 방법__ 으로 구현 가능하다.
* 알고리즘 대회에서는 __재귀적인 방법__ 을 많이 이용한다.
  * __재귀적 방법은__ 컴퓨터의 스택 프레임을 이용하는것이기 때문에 내부적으로 스택을 이용하는것이다.
* __장점__
  1. 단지 현 경로상의 노드들만을 기억하면 되므로 저장공간의 수요가 비교적 적다.
  2. 목표노드가 깊은 단계에 있을 경우 해를 빨리 구할 수 있다.
* __단점__
  1. __해가 없는 경로에 깊이 빠질 가능성이 있다.__ 따라서 실제의 경우 미리 지정한 임의의 깊이까지만 탐색하고 목표노드를 발견하지 못하면 다음의 경로를 따라 탐색하는 방법이 유용할 수 있다.
  2. __얻어진 해가 최단 경로가 된다는 보장이 없다.__ 이는 목표에 이르는 경로가 다수인 문제에 대해 깊이우선 탐색은 해에 다다르면 탐색을 끝내버리므로, 이때 얻어진 해는 최적이 아닐 수 있다는 의미이다.

## [참고]BFS(Breadth First Search)
* 다차원 배열에서 각 칸을 방문할 때 너비를 우선으로 방문하는 알고리즘

## DFS 동작 예시
1. 시작하는 칸을 스택에 넣고 방문했다는 표시를 남김
2. 스택에서 원소를 꺼내어 그 칸과 상하좌우로 인접한 칸에 대해 3번을 진행
3. 해당 칸을 이전에 방문했다면 아무 것도 하지 않고, 처음으로 방문했다면 방문했다는 표시를 남기고 해당 칸을 스택에 삽입
4. 스택이 빌 때 까지 2번을 반복

> 모든 칸이 스택에 1번씩 들어가므로 __시간복잡도는 칸이 N개일때 O(N)__

## 깊이 우선 탐색(Depth First Search)의 알고리즘 예시
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/dfs1.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/dfs1.JPG" title="Check out the image">
</a>

* 부모노드로 되돌아오는 과정을 __백트래킹(backtracking)이라__ 한다.

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/dfs2.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/dfs2.JPG" title="Check out the image">
</a>

* Step 6에서 더 이상 방문할 노드가 없으므로 스택에서 제거하고 백트래킹(backtracking)하여 방문하지 않은 노드를 방문한다.

## C++ STL Library를 사용한 DFS 구현1
* 깊이 우선 탐색(Depth First Search)을 재귀적인 방법을 이용해서 구현

{% highlight javascript %}
#include <iostream>
#include <vector>

using namespace std;

// 노드의 개수 
int num = 7;
// 방문처리를 위한 배열 
int c[7];
// 7개의 인접한 노드를 넣어주기위한 백터 
vector<int> a[8];

void dfs(int x){
	if(c[x]) return; 
	c[x] = true;
	cout << x << ' ';
	// 인접노드를 반복적으로 방문한다. 
	for(int i = 0; i < a[x].size(); i++){
		int y = a[x][i];
		dfs(y);	
	}
}
int main(void){
	
	a[1].push_back(2);
	a[2].push_back(1);
	
	a[1].push_back(3);
	a[3].push_back(1);
	
	a[2].push_back(3);
	a[3].push_back(2);
	
	a[2].push_back(4);
	a[4].push_back(2);
	
	a[2].push_back(5);
	a[5].push_back(2);
	
	a[3].push_back(6);
	a[6].push_back(3);
	
	a[3].push_back(7);
	a[7].push_back(3);
	
	a[4].push_back(5);
	a[5].push_back(4);
	
	a[6].push_back(7);
	a[7].push_back(6);
	
	dfs(1);
	
	return 0;
}
{% endhighlight %}

## DFS 구현2

```cpp
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

## DFS와 BFS 방문순서 차이

![algorithm-dfs_bfs_move]({{ site.baseurl }}/images/Algorithm/algorithm-dfs_bfs_move.JPG)

## Reference
* [[바킹독의 실전 알고리즘] 0x0A강 - DFS](https://www.youtube.com/watch?v=93jy2yUYfVE)
* [17강 - 깊이 우선 탐색](https://www.youtube.com/watch?v=l0Rsu7dziws&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=17)
* [[알고리즘] 깊이 우선 탐색(DFS)이란](https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html)
* [위키 - 깊이 우선 탐색](https://ko.wikipedia.org/wiki/%EA%B9%8A%EC%9D%B4_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89)
---
title: 너비 우선 탐색(BFS, Breadth First Search)
category: Algorithm
date: 2019-07-22 16:32:30
lastmod: 2020-09-16 00:32:30
comments: true
order: 11
---

## 너비 우선 탐색(BFS, Breadth First Search)
* 다차원 배열에서 각 칸을 방문할 때 __너비를 우선으로 방문하는 알고리즘__
* BFS는 방문한 노드들을 차례로 저장하고 꺼낼 수 있는 __자료구조인 큐(Queue)를 사용__ 한다.
* BFS는 __다차원 배열의 거리 계산 및 순회하는데 사용된다.__
* 너비 우선 탐색(Breadth-first search, BFS)은 __'맹목적 탐색방법'__ 의 하나이다.<br>
  + __맹목적 탐색__ 이란, 이미 정해진 순서에 따라 상태 공간 그래프를 점차 형성해 가면서 해를 탐색하는 방법을 말한다.
* __루트 노드 (혹은 다른 임의의 노드)__ 에서 시작해 __인접한 노드를__ 먼저 탐색하는 방법.
* 더 이상 방문하지 않은 정점이 없을 때까지 방문하지 않은 모든 정점들에 대해서도 너비 우선 검색을 적용한다.

> __두 노드 사이의 최단 경로__ 혹은 __임의의 경로를__ 찾고 싶을 때 이 방법을 선택한다.

* __재귀적으로__ 동작하지 않는다.
* 'Prim', 'Dijkstra' 알고리즘과 유사하다.
* 출발노드에서 목표노드까지의 __최단 길이 경로를 보장__ 한다.
* 그래프 탐색의 경우 어떤 노드를 방문했었는지 여부를 반드시 검사 해야한다. 그렇지 않으면 __무한루프에__ 빠질 위험이 있다.(단점3)
* __단점__
  1. __경로가 매우 길 경우__ 에는 탐색 가지가 급격히 증가함에 따라 보다 많은 기억 공간을 필요로 하게 된다.
  2. __해가 존재하지 않는다면__ 유한 그래프(finite graph)의 경우에는 모든 그래프를 탐색한 후에 실패로 끝난다.
  3. __무한 그래프(infinite graph)의__ 경우에는 결코 해를 찾지도 못하고, 끝내지도 못한다.


![algorithm-animated_bfs]({{ site.baseurl }}/images/Algorithm/algorithm-animated_bfs.gif)

## 너비 우선 탐색(Breath First Search)의 알고리즘 예시

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/bfs1.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/bfs1.JPG" title="Check out the image">
</a>

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/bfs2.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/bfs2.JPG" title="Check out the image">
</a>

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/bfs3.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/bfs3.JPG" title="Check out the image">
</a>

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/bfs4.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/bfs4.JPG" title="Check out the image">
</a>

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/bfs5.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/bfs5.JPG" title="Check out the image">
</a>

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/bfs6.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/bfs6.JPG" title="Check out the image">
</a>


## BFS 구현

아래와 같은 형태의 그래프가 주어질때 BFS를 구현 해보겠습니다.

```
1─2─5
|/ \│
3   4
|\
6─7
```

#### C++ BFS 구현 예시1
__인접 리스트__ 가 입력 값으로 주어진 상황에서 C++ STL Library로 구현한 BFS 코드입니다.

```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

// 1~7번 노드의 방문 처리 하기 위한 배열 
int c[7];
vector<int> a[8];
void bfs(int start){
	queue<int> q;
	q.push(start);
	c[start] = true;
	//search
	while(!q.empty()){
		int x = q.front();
		q.pop();
		printf("%d ", x);
		for(int i = 0; i < a[x].size(); i++){
			int y = a[x][i];
			// 방문한 상태가 아니라면 큐에 넣어준다.
			if(!c[y]){
				q.push(y);
				c[y] = true; 
			}
		}
	}
}
int main(void){
	int input[8][2] = {
		{1,2},{1,3},{2,3},{2,5},{2,4},{3,6},{4,5},{6,7}
		};
	//data input
	for (int i = 0; i < 8; i++) {	
		a[input[i][0]].push_back(input[i][1]);
		a[input[i][1]].push_back(input[i][0]);
	}
	
	bfs(1);
	
	return 0;
}
```

#### C++ BFS 구현 예시2
__인접 행렬__ 이 입력 값으로 주어진 상황에서 C++로 구현한 BFS 코드입니다.

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

#### Java BFS 구현 예시1
__인접 리스트__ 가 입력 값으로 주어진 상황에서 Java Collection으로 구현한 BFS 코드입니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;

class Main {
	static int[][] input = { 
		{1,2},{1,3},{2,3},{2,5},{2,4},{3,6},{4,5},{6,7}
		};
	static LinkedList<Integer>[] board = new LinkedList[8];
	static boolean[] visit = new boolean[8];

	public static void bfs(int start) {
		Queue<Integer> q = new LinkedList<>();
		q.add(start);
		visit[start] = true;

		while(!q.isEmpty()) {
			int cur = q.poll();
			System.out.print(cur + " ");
			for (int i = 0; i < board[cur].size(); i++) {
				int next = board[cur].get(i);
				if(!visit[next]) {
					q.add(next);
					visit[next] = true;
				}
			}
		}
	}
	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		//init
		for (int i = 1; i <= 7; i++) {
			board[i] = new LinkedList<>();
		}
		//input
		for (int i = 0; i < input.length; i++) {
			board[input[i][0]].add(input[i][1]);
			board[input[i][1]].add(input[i][0]);
		}
		bfs(1);
		br.close();
	}
}
```

## BFS 문제 유형(+응용 유형)
* Flood Fill
* 거리 측정
* 시작점이 여러 개일 때
* 시작점이 두 종류일 때
* 1차원에서의 BFS

## 주의할점
* 시작점에 방문했다는 표시를 남기지 않는다.
* 큐에 넣을 때 방문했다는 표시를 하는 대신 큐에서 빼낼 때 방문했다는 표시를 남겼다.
* 이웃한 원소가 범위를 벗어났는지에 대한 체크를 잘못했다.

## BFS 연습문제
* [BOJ 1926: 그림](https://www.acmicpc.net/problem/1926)
* [BOJ 2178: 미로 탐색](https://www.acmicpc.net/problem/2178)
* [BOJ 7576: 토마토](https://www.acmicpc.net/problem/7576)
* [BOJ 4179: 불](https://www.acmicpc.net/problem/4179)
* [BOJ 1697: 숨바꼭질](https://www.acmicpc.net/problem/1697)

## Reference
* [[바킹독의 실전 알고리즘] 0x09강 - BFS](https://www.youtube.com/watch?v=ftOmGdm95XI)
* [commons.wikimedia.org](https://commons.wikimedia.org/wiki/File:Animated_BFS.gif)
* [[알고리즘] 너비 우선 탐색(BFS)이란](https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html)
* [15. 너비 우선 탐색(BFS)](https://blog.naver.com/ndb796/221230944971)
* [위키 - 너비 우선 탐색](https://ko.wikipedia.org/wiki/%EB%84%88%EB%B9%84_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89)
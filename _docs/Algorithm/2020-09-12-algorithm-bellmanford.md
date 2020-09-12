---
title: "벨만포드(Bellman Ford) 알고리즘"
category: Algorithm
date: 2020-09-12 00:34:59
comments: true
order: 30
---


## 벨만포드(Bellman Ford) 알고리즘 이란?
한 정점에서 다른 모든 정점으로 가는데 걸리는 __최소비용__ 을 구하는 알고리즘입니다.

위의 설명은 다익스트라 알고리즘과 동일합니다. 그렇다면 __벨만포드 알고리즘과 다익스트라 알고리즘의 차이점은 무엇__ 인지 의문이 생기게 됩니다. 

__다익스트라 알고리즘__ 의 경우 __"양의 가중치만 있을 때"__ 사용이 가능한 알고리즘입니다. 그 이유는 그래프 상에 음의 가중치가 존재한다면 다익스트라의 규칙이 깨지기 때문입니다.

__벨만포드 알고리즘__ 은 __"음의 가중치가 존재"__ 할때 사용하는 알고리즘입니다. 또한, 음의 가중치가 존재할때 __음의 사이클의 존재여부를 판단__ 할 수 있습니다. 

따라서, 다익스트라와 벨만포드의 차이점은 __음의 가중치 존재 여부__ 라고 할 수 있습니다.

## 벨만포드 알고리즘 동작 원리
* 모든 간선을 탐색하면서, __"한번이라도 계산된 정점"__ 이라면 거리 비교를 통해 최소 비용으로 업데이트 합니다.
* N개의 노드가 있을때 N-1번 반복합니다.
  + 한 정점에서 __모든 정점으로 가는데 걸리는 최소비용__ 을 구해야 하기 때문입니다. 


## 벨만포드 알고리즘 구현

```c++
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x7f7f7f7f;

int N;
int visit[10];
void bellmanFord(vector<vector<int>> &board) {
    visit[1] = 0;
    //n-1 반복
    for (int i = 0; i < N-1; i++) {
        for (int j = 0; j < board.size(); j++) {
            int from = board[j][0];
            int to = board[j][1];
            int cost = board[j][2];
            if(visit[from] == INF) continue;
            if(visit[to] > visit[from] + cost) {
                visit[to] = visit[from] + cost;
            }
        }
    }
    //사이클 검증
    for (int i = 0; i < board.size(); i++) {
        int from = board[i][0];
        int to = board[i][1];
        int cost = board[i][2];
        if(visit[from] == INF) continue;
        if(visit[to] > visit[from] + cost) {
            cout << "Exist Cycle UU!\n";
            return;
        }
    }
    cout << "No Cycle!\n";
}

void solve(int n, vector<vector<int>> &board) {
    N = n;
    fill(visit,visit+n,INF);
    bellmanFord(board);
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    //n=노드 개수, {from, to, cost}
    solve(5, { {1,2,1},{2,3,2},{2,4,1},{4,5,-1},{5,2,-2} });//Cycle!
    /*
    1 -(1)→ 2 -(2)→ 3
          ↗ \
       (-2)  (1)
       /      ↘
      5 ←(-1)- 4
    */
    solve(3, { {1,2,1},{2,3,2} });//No Cycle!
    /*
        1 -(1)→ 2 -(2)→ 3
    */
    return 0;
}
```

## Reference
* [yabmoons.tistory.com](https://yabmoons.tistory.com/365?category=838490)
---
title: 연결 리스트(Linked List)
category: DataStructure
date:   2020-06-04 00:30:59
comments: true
order: 5
---

## 연결 리스트의 성질
* k번째 원소를 확인/변경 : O(k)
* 임의의 위치에 원소를 추가/제거 : O(1)
* 원소들이 메모리 상에 연속해있지 않아 Cache hit rate가 낮지만 할당이 다소 쉽다.

## 연결 리스트의 종류
* 단일 연결 리스트(Singly Linked List)
* 이중 연결 리스트(Doubly Linked List)
* 원형 연결 리스트(Circular Linked List)

## 배열 vs 연결 리스트

|   |  <center>배열</center> |  <center>연결 리스트</center> |
|:--------:|:--------:|:--------:|
|k번째 원소의 접근|O(1)|O(k)|
|임의 위치에 원소 추가/제거|O(N)|O(1)|
|메모리 상의 배치|연속|불연속|
|추가적으로 필요한 공간<br/>(Overhead)| - | O(N) |


## 연결 리스트 구현(꼼수)

```cpp
#include <bits/stdc++.h>
using namespace std;
#define idx 10

const int MAX = 1000005;
int dat[MAX], pre[MAX], nxt[MAX];
int unused = 1;


void insert(int addr, int num) {
    dat[unused] = num;
    pre[unused] = addr;
    nxt[unused] = nxt[addr];
    if(nxt[addr] != -1) pre[nxt[addr]] = unused;
    nxt[addr] = unused;
    unused++;
}

void erase(int addr) {
    nxt[pre[addr]] = nxt[addr];
    if(nxt[addr] != -1) pre[nxt[addr]] = pre[addr];
}

void traverse(){
    int cur = nxt[0];
    while (cur != -1)
    {
        cout << dat[cur] << ' ';
        cur = nxt[cur];
    }
    cout << "\n";
}

void log() {
    cout << "==========\ndat= ";
    for(int i = 0; i < idx; i++) {
        cout << dat[i] << "  ";
    }
    cout << "\npre= ";
    for(int i = 0; i < idx; i++) {
        cout << pre[i] << "  ";
    }
    cout << "\nnxt= ";
    for(int i = 0; i < idx; i++) {
        cout << nxt[i] << "  ";
    }
    cout << "\n";
}
int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    
    fill(pre, pre+MAX, -1);
    fill(nxt, nxt+MAX, -1);

    insert(0, 3);
    log();
    insert(1, 5);
    log();
    insert(2, 2);
    log();
    insert(3, 1);
    log();
    insert(3, 7);
    log();
    traverse();
    erase(2);
    log();
    traverse();
    
    return 0;
}
```

## 연결 리스트 STL

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(void) {
  list<int> L = {1,2}; // 1 2
  list<int>::iterator t = L.begin(); // t는 1을 가리키는 중
  L.push_front(10); // 10 1 2
  cout << *t << '\n'; // t가 가리키는 값 = 1을 출력
  L.push_back(5); // 10 1 2 5
  L.insert(t, 6); // t가 가리키는 곳 앞에 6을 삽입, 10 6 1 2 5
  t++; // t를 1칸 앞으로 전진, 현재 t가 가리키는 값은 2
  t = L.erase(t); // t가 가리키는 값을 제거, 그 다음 원소인 5의 위치를 반환
                  // 10 6 1 5, t가 가리키는 값은 5
  cout << *t << '\n'; // 5
  for(auto i : L) cout << i << ' ';
  cout << '\n';
  for(list<int>::iterator it = L.begin(); it != L.end(); it++)
    cout << *it << ' ';
}
```

## Question
__1) 원형 연결 리스트 내의 임의의 노드 하나가 주어졌을 때 해당 List의 길이를 효율적으로 구하는 방법은?__

동일한 노드가 나올 때 까지 꼐속 당므 노드로 가면 됨, __공간복잡도 O(1), 시간복잡도 O(n)__

__2) 중간에 만나는 두 연결 리스트의 시작점들이 주어졌을 때 만나는 지점을 구하는 방법?__

일단 두 시작점 각각에 대해 끝까지 진행시켜서 각각의 길이를 구함. 그 후 다시 두 시작점으로 돌아와서 더 긴 쪽을 둘의 차이만큼 앞으로 먼저 이동시켜놓고 두 시작점이 만날 때 까지 두 시작점을 동시에 한 칸씩 전진시키면 됨. __공간복잡도O(1), 시간복잡도O(A+B)__

__3) 연결 리스트 안에 사이클이 있는지 판단하라__

1->2->3-> __4__ ->5->6->__4__

Floyd's cycle-finding algorithm, 공간복잡도 O(1), 시간복잡도 O(N)



# References
* [바킹독의 실전 알고리즘-0x04, 연결 리스트](https://www.youtube.com/watch?v=C6MX5u7r72E)
---
title: 덱(deque)
category: DataStructure
date:   2020-06-04 00:30:59
comments: true
order: 4
---

## 덱(deque)의 성질
* 앞과 뒤에서 삽입/삭제가 가능한 자료구조이다.
* 원소의 추가가 O(1)
* 원소의 제거가 O(1)
* 제일 앞/뒤의 원소 확인이 O(1)
* 제일 앞/뒤가 아닌 나머지 원소들의 확인/변경이 원칙적으로 불가능
  + STL deque에서는 인덱스로 원소에 접근할 수 있다.
* __선형구조__ 의 자료구조이다.

## 배열을 이용한 덱 구현

```cpp
// #include <bits/stdc++.h>
#include <iostream>

using namespace std;
#define ll long long

const int mx = 1000005;
int dat[2*mx+1];
int head = mx, tail = mx;

void push_front(int x) { dat[--head] = x; }
void push_back(int x) { dat[tail++] = x; }
void pop_front() { head++; }
void pop_back() { tail--; }
int front() {
    return dat[head];
}
int back() {
    return dat[tail-1];
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    
    return 0;
}
```

## STL을 이용한 덱 구현

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(void){
  deque<int> DQ;
  DQ.push_front(10); // 10
  DQ.push_back(50); // 10 50
  DQ.push_front(24); // 24 10 50
  for(auto x : DQ)cout<<x;
  cout << DQ.size() << '\n'; // 3
  if(DQ.empty()) cout << "DQ is empty\n";
  else cout << "DQ is not empty\n"; // DQ is not empty
  DQ.pop_front(); // 10 50
  DQ.pop_back(); // 10
  cout << DQ.back() << '\n'; // 10
  DQ.push_back(72); // 10 72
  cout << DQ.front() << '\n'; // 10
  DQ.push_back(12); // 10 72 12
  DQ[2] = 17; // 10 72 17
  DQ.insert(DQ.begin()+1, 33); // 10 33 72 17
  DQ.insert(DQ.begin()+4, 60); // 10 33 72 17 60
  for(auto x : DQ) cout << x << ' ';
  cout << '\n';
  DQ.erase(DQ.begin()+3); // 10 33 72 60
  cout << DQ[3] << '\n'; // 60
  DQ.clear(); // DQ의 모든 원소 제거
}
```

* 덱은 vector과 달리 원소들이 메모리 상에 __연속하게 배치되어 있지 않는다.__

## References
* [바킹독의 실전 알고리즘-0x07, 덱](https://www.youtube.com/watch?v=0mEzJ4S1d8o)